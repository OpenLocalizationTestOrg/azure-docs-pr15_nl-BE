<properties 
    pageTitle="Service Bus en Python met AMQP 1.0 | Microsoft Azure"
    description="Service Bus van Python met AMQP gebruikt."
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

# <a name="using-service-bus-from-python-with-amqp-10"></a>Bus Service van Python gebruiken met AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton-Python is een Python language binding met Proton-C; dat wil zeggen, is Proton Python geïmplementeerd als een wrapper rond een motor die is geïmplementeerd in C.

## <a name="download-the-proton-client-library"></a>De bibliotheek Proton-client downloaden

Proton-C en de bijbehorende Bindingen (met inbegrip van Python) kunt u downloaden van [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). De download is in de vorm van broncode. Volg de instructies in het gedownloade pakket voor het opbouwen van de code.

Houd er rekening mee dat op het moment van schrijven van dit in de Proton-C de SSL-ondersteuning alleen beschikbaar voor Linux-systemen is. Aangezien Azure Service Bus het gebruik van SSL vereist, kunnen Proton-C (en de bindingen taal) alleen worden gebruikt voor access-Service Bus van Linux op dit moment. Werk inschakelen Proton-C met SSL op Windows is bezig zo controle terug vaak voor updates.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-python"></a>Werken met wachtrijen, onderwerpen en Python-abonnementen Service Bus

De volgende code toont hoe u berichten verzenden en ontvangen van een Service Bus berichten entiteit.

### <a name="send-messages-using-proton-python"></a>Berichten verzenden met Proton-Python

De volgende code toont hoe u een bericht verzenden naar een Service Bus berichten entiteit.

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### <a name="receive-messages-using-proton-python"></a>Ontvangen berichten via Proton-Python

De volgende code toont hoe u een bericht ontvangt van een Service Bus berichten entiteit.

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## <a name="messaging-between-net-and-proton-python"></a>Berichten tussen .NET en Proton-Python

### <a name="application-properties"></a>Eigenschappen van toepassingen

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python op Bus-API's van .NET Service

Proton-Python berichten toepassingseigenschappen van de volgende typen ondersteuning: **int**, **long**, **float**, **uuid**, **bool**, **string**. De volgende Python code ziet u hoe eigenschappen instellen voor een bericht met behulp van elk type eigenschap.

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

In de API Service Bus .NET worden toepassing berichteigenschappen vervoerd in de **Properties** -collectie van [BrokeredMessage][]. De volgende code ziet u hoe de eigenschappen van een bericht ontvangen van een client Python.

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

In de volgende tabel wordt de soorten Python toegewezen aan de .NET-soorten.

| Python (Type eigenschap) | .NET (Type eigenschap) |
|----------------------|--------------------|
| int                  | int                |
| Float                | dubbel             |
| lange                 | Int64              |
| UUID                 | GUID               |
| BOOL                 | BOOL               |
| tekenreeks               | tekenreeks             |

#### <a name="service-bus-net-apis-to-proton-python"></a>Service Bus .NET API's Proton-Python

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
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

De volgende Python code ziet u hoe de eigenschappen van een bericht ontvangen van een client Service Bus .NET.

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

In de volgende tabel worden de soorten .NET aan de soorten Python toegewezen.

| .NET (Type eigenschap) | Python (Type eigenschap) | Notities                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| byte               | int                  | -                                                                                                                                                                     |
| SByte              | int                  | -                                                                                                                                                                     |
| CHAR               | CHAR                 | Klasse proton-Python                                                                                                                                                 |
| korte              | int                  | -                                                                                                                                                                     |
| USHORT             | int                  | -                                                                                                                                                                     |
| int                | int                  | -                                                                                                                                                                     |
| uint               | int                  | -                                                                                                                                                                     |
| lange               | int                  | -                                                                                                                                                                     |
| ULONG              | lange                 | Klasse proton-Python                                                                                                                                                 |
| Float              | Float                | -                                                                                                                                                                     |
| dubbel             | Float                | -                                                                                                                                                                     |
| decimaal            | Tekenreeks               | Achter de komma wordt momenteel niet ondersteund met Proton.                                                                                                                     |
| BOOL               | BOOL                 | -                                                                                                                                                                     |
| GUID               | UUID                 | Klasse proton-Python                                                                                                                                                 |
| tekenreeks             | tekenreeks               | -                                                                                                                                                                     |
| Datum/tijd           | tijdstempel            | Klasse proton-Python                                                                                                                                                 |
| DateTimeOffset     | DescribedType        | Toegewezen aan AMQP type DateTimeOffset.UtcTicks:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| TimeSpan           | DescribedType        | Toegewezen aan AMQP type Timespan.Ticks:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType        | Toegewezen aan AMQP type Uri.AbsoluteUri:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-properties"></a>Standaardeigenschappen

De volgende tabellen ziet u de toewijzing tussen de Proton Python standaardbericht eigenschappen en de [BrokeredMessage][] het standaardbericht.

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python op Bus-API's van .NET Service

| Proton-Python        | Bus service .NET         | Notities                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| duurzame              | n.v.t.                      | Bus-service ondersteunt alleen duurzame berichten.               |
| prioriteit             | n.v.t.                      | Bus-service ondersteunt alleen de prioriteit van een bericht.      |
| TTL                  | Message.TimeToLive       | Conversie van Proton Python TTL wordt gedefinieerd in milliseconden. |
| eerste\_overnemende partij      | n.v.t.                      | -                                                           |
| levering\_tellen      | n.v.t.                      | -                                                           |
| ID                   | Message.MessageID        | -                                                           |
| gebruiker\_id             | n.v.t.                      | -                                                           |
| adres              | Message.To               | -                                                           |
| Onderwerp              | Message.Label            | -                                                           |
| antwoord op\_aan            | Message.ReplyTo          | -                                                           |
| correlatie\_id      | Message.CorrelationID    | -                                                           |
| inhoud\_type        | Message.ContentType      | -                                                           |
| inhoud\_codering    | n.v.t.                      | -                                                           |
| uiterste\_tijd         | n.v.t.                      | -                                                           |
| maken van\_tijd       | n.v.t.                      | -                                                           |
| groep\_id            | Message.SessionId        | -                                                           |
| groep\_reeks      | n.v.t.                      | -                                                           |
| antwoord op\_voor\_groep\_id | Message.ReplyToSessionId | -                                                           |
| indeling               | n.v.t.                      | -                                                           |

| Bus service .NET        | Proton                       | Notities                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| ContentType             | Message.content\_type        | -                                                           |
| CorrelationId           | Message.Correlation\_id      | -                                                           |
| EnqueuedTimeUtc         | n.v.t.                          | -                                                           |
| Label                   | Message.Subject              | -                                                           |
| MessageId               | Message.id                   | -                                                           |
| ReplyTo                 | Message.Reply\_aan            | -                                                           |
| ReplyToSessionId        | Message.Reply\_te\_groep\_id | -                                                           |
| ScheduledEnqueueTimeUtc | n.v.t.                          | -                                                           |
| Sessie-id               | Message.group\_id            | -                                                           |
| TimeToLive              | Message.TTL                  | Conversie van Proton Python TTL wordt gedefinieerd in milliseconden. |
| Aan                      | Message.Address              | -                                                           |

## <a name="next-steps"></a>Volgende stappen

Wilt u meer informatie? Ga naar de volgende koppelingen:

- [Overzicht Service Bus AMQP]
- [AMQP in Bus Service voor Windows Server]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP in Bus Service voor Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx

[Overzicht Service Bus AMQP]: service-bus-amqp-overview.md
