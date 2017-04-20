<properties 
    pageTitle="Service Bus en PHP met AMQP 1.0 | Microsoft Azure"
    description="Service Bus van PHP met AMQP gebruikt."
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

# <a name="using-service-bus-from-php-with-amqp-10"></a>Service Bus van PHP gebruikt met AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton-PHP is een PHP-taal-binding met de Proton-C; dat wil zeggen, is Proton PHP geïmplementeerd als een wrapper rond een motor die is geïmplementeerd in C.

## <a name="downloading-the-proton-client-library"></a>De bibliotheek Proton-client downloaden

Proton-C en de bijbehorende Bindingen (inclusief PHP) kunt u downloaden van [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). De download is in de vorm van broncode. Volg de instructies in het gedownloade pakket voor het opbouwen van de code.

> [AZURE.IMPORTANT] De SSL-ondersteuning in de Proton-C is alleen beschikbaar voor Linux-systemen op het moment van schrijven van dit. Aangezien Azure Service Bus het gebruik van SSL vereist, kunnen Proton-C (en de bindingen taal) alleen worden gebruikt voor access-Service Bus van Linux op dit moment. Proton-C met SSL op Windows inschakelen zijn bezig, zo controle terug vaak voor updates.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-php"></a>Werken met wachtrijen, onderwerpen en PHP-abonnementen Service Bus

De volgende code toont hoe u berichten verzenden en ontvangen van een Service Bus berichten entiteit.

### <a name="sending-messages-using-proton-php"></a>Verzenden van berichten met Proton-PHP

De volgende code toont hoe u een bericht verzenden naar een Service Bus berichten entiteit.

```
$messenger = new Messenger();
$message = new Message();
$message->address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";

$message->body = "This is a text string";
$messenger->put($message);
$messenger->send();
```

### <a name="receiving-messages-using-proton-php"></a>Ontvangen berichten met Proton-PHP

De volgende code toont hoe u een bericht ontvangt van een Service Bus berichten entiteit.

```
$messenger = new Messenger();
$address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";
$messenger->subscribe($address);

$messenger->start();
$messenger->recv(1);

if($messenger->incoming())
{
   $message = new Message();
   $messenger->get($message);      
}

$messenger->stop();
```

## <a name="messaging-between-net-and-proton-php"></a>Berichten tussen .NET en Proton-PHP

### <a name="application-properties"></a>Eigenschappen van toepassingen

#### <a name="protonphp-to-service-bus-net-apis"></a>ProtonPHP op Bus-API's van .NET Service

Proton PHP berichten toepassingseigenschappen van de volgende typen ondersteuning: **integer**, **dubbele**, **Boolean**, **tekenreeks**en **object**. De volgende PHP-code ziet u hoe eigenschappen instellen voor een bericht met behulp van elk type eigenschap.

```
$message->properties["TestInt"] = 1;    
$message->properties["TestDouble"] = 1.5;      
$message->properties["TestBoolean"] = False;
$message->properties["TestString"] = "Service Bus";    
$message->properties["TestObject"] = new UUID("1234123412341234");   
```

Berichteigenschappen van toepassing zijn in de Service Bus .NET API's die in de **Properties** -collectie van [BrokeredMessage][]. De volgende code ziet u hoe de eigenschappen van een bericht ontvangen van een PHP-client.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}if (message.Properties.Keys.Count > 0)
{
foreach (string name in message.Properties.Keys)
{
  Object value = message.Properties[name];
  Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
}
Console.WriteLine();
}
```

In de volgende tabel wordt de PHP-soorten toegewezen aan de .NET-soorten.

| PHP (Type eigenschap) | .NET (Type eigenschap) |
|-------------------|--------------------|
| geheel getal           | int                |
| dubbel            | dubbel             |
| Boole-waarde           | BOOL               |
| tekenreeks            | tekenreeks             |
| object            | Object             |

#### <a name="service-bus-net-apis-to-php"></a>Service Bus .NET API's voor PHP

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

De volgende PHP-code ziet u hoe de eigenschappen van een bericht ontvangen van een client Service Bus .NET.

```
if ($message->properties != null)
{
  foreach($message->properties as $key => $value)
  {
    printf("-- %s : %s (%s) \n", $key, $value, gettype($value));                       
  }         
}
```

In de volgende tabel wordt de soorten .NET toegewezen aan de PHP-soorten.

| .NET (Type eigenschap) | PHP (Type eigenschap) | Notities                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| byte               | geheel getal           | -                                                                                                                                                                     |
| SByte              | geheel getal           | -                                                                                                                                                                     |
| CHAR               | CHAR              | Proton-PHP-klasse                                                                                                                                                    |
| korte              | geheel getal           | -                                                                                                                                                                     |
| USHORT             | geheel getal           | -                                                                                                                                                                     |
| int                | geheel getal           | -                                                                                                                                                                     |
| uint               | Geheel getal           | -                                                                                                                                                                     |
| lange               | geheel getal           | -                                                                                                                                                                     |
| ULONG              | geheel getal           | -                                                                                                                                                                     |
| Float              | dubbel            | -                                                                                                                                                                     |
| dubbel             | dubbel            | -                                                                                                                                                                     |
| decimaal            | tekenreeks            | Achter de komma wordt momenteel niet ondersteund met Proton.                                                                                                                     |
| BOOL               | Boole-waarde           | -                                                                                                                                                                     |
| GUID               | UUID              | Proton-PHP-klasse                                                                                                                                                    |
| tekenreeks             | tekenreeks            | -                                                                                                                                                                     |
| Datum/tijd           | geheel getal           | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | Toegewezen aan AMQP type DateTimeOffset.UtcTicks:<type name="datetime-offset" class=restricted source="long"><descriptor name="com.microsoft:datetime-offset" /></type> |
| TimeSpan           | DescribedType     | Toegewezen aan AMQP type Timespan.Ticks:<type name="timespan" class=restricted source="long"><descriptor name="com.microsoft:timespan" /></type>                        |
| URI                | DescribedType     | Toegewezen aan AMQP type Uri.AbsoluteUri:<type name="uri" class=restricted source="string"><descriptor name="com.microsoft:uri" /></type>                               |

### <a name="standard-properties"></a>Standaardeigenschappen

De volgende tabellen ziet u de toewijzing tussen de Proton PHP standaardbericht eigenschappen en de [BrokeredMessage][] het standaardbericht.

| Proton-PHP           | Bus service .NET         | Notities                                                    |
|----------------------|--------------------------|----------------------------------------------------------|
| Duurzame              | n.v.t.                      | Bus-service ondersteunt alleen duurzame berichten.          |
| Prioriteit             | n.v.t.                      | Bus-service ondersteunt alleen de prioriteit van een bericht. |
| TTL                  | Message.TimeToLive       | Conversie van Proton PHP-TTL wordt gedefinieerd in milliseconden.   |
| eerste\_overnemende partij      | -                          | -                                                          |
| levering\_tellen      | -                          | -                                                          |
| ID                   | Message.Id               | -                                                          |
| gebruiker\_id             | -                          | -                                                          |
| Adres              | Message.To               | -                                                          |
| Onderwerp              | Message.Label            | -                                                          |
| antwoord op\_aan            | Message.ReplyTo          | -                                                          |
| correlatie\_id      | Message.CorrelationId    | -                                                          |
| inhoud\_type        | Message.ContentType      | -                                                          |
| inhoud\_codering    | n.v.t.                      | -                                                          |
| uiterste\_tijd         | Message.ExpiresAtUTC     | -                                                          |
| maken van\_tijd       | n.v.t.                      | -                                                          |
| groep\_id            | Message.SessionId        | -                                                          |
| groep\_reeks      | -                          | -                                                          |
| antwoord op\_voor\_groep\_id | Message.ReplyToSessionId | -                                                          |
| Indeling               | n.v.t.                      | -

#### <a name="service-bus-net-apis-to-proton-php"></a>Service Bus .NET API's Proton PHP

| Bus service .NET        | Proton-PHP                                             | Notities                                                  |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------|
| ContentType             | Bericht -\>inhoud\_type                                | -                                                        |
| CorrelationId           | Bericht -\>correlatie\_id                              | -                                                        |
| EnqueuedTimeUtc         | Bericht -\>aantekeningen [x-opt-in wachtrij-time]             | -                                                        |
| Label                   | Bericht -\>onderwerp                                      | -                                                        |
| MessageId               | Bericht -\>id                                           | -                                                        |
| ReplyTo                 | Bericht -\>antwoord\_aan                                    | -                                                        |
| ReplyToSessionId        | Bericht -\>antwoord\_te\_groep\_id                         | -                                                        |
| ScheduledEnqueueTimeUtc | Bericht -\>aantekeningen ["x-opt-gepland-plaatsen-time'] | -                                                        |
| Sessie-id               | Bericht -\>groep\_id                                    | -                                                        |
| TimeToLive              | Bericht -\>ttl                                          | Conversie van Proton PHP-TTL wordt gedefinieerd in milliseconden. |
| Aan                      | Bericht -\>adres                                      | -                                                        |

## <a name="next-steps"></a>Volgende stappen

Wilt u meer informatie? Ga naar de volgende koppelingen:

- [Overzicht Service Bus AMQP]
- [AMQP in Bus Service voor Windows Server]


[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP in Bus Service voor Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
[Overzicht Service Bus AMQP]: service-bus-amqp-overview.md
