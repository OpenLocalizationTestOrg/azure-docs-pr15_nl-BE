<properties
    pageTitle="Service Bus onderwerpen gebruiken met .NET | Microsoft Azure"
    description="Informatie over Service Bus onderwerpen en abonnementen gebruiken met .NET in Azure. Codevoorbeelden zijn geschreven voor .NET-toepassingen."
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
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Het gebruik van de Service Bus onderwerpen en abonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dit artikel wordt beschreven hoe u Service Bus onderwerpen en abonnementen. De monsters zijn geschreven in C# en het .NET API's gebruiken. De scenario's die zijn maken van onderwerpen en abonnementen, abonnement filters maken, verzenden van berichten naar een onderwerp, ontvangen van een abonnement en onderwerpen en abonnementen verwijderen. Zie de sectie [volgende stappen](#next-steps) voor meer informatie over onderwerpen en abonnementen.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>De toepassing configureren voor gebruik van de Service Bus

Wanneer u een toepassing die gebruikmaakt van de Service Bus maakt, moet u een verwijzing toevoegen aan de assembly Service Bus en nemen de bijbehorende naamruimten. De eenvoudigste manier om dit te doen is om de juiste [NuGet](https://www.nuget.org) downloaden.

## <a name="get-the-service-bus-nuget-package"></a>Het pakket Service Bus NuGet ophalen

De [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is de eenvoudigste manier om de Service Bus-API en het configureren van uw toepassing met de benodigde Service Bus afhankelijkheden. De Service Bus NuGet om pakket te installeren in uw project, het volgende doen:

1.  In Solution Explorer met de rechtermuisknop op **References**, klik op **Beheren NuGet pakketten**.
2.  'Service Bus' zoekt en selecteert u het item **Microsoft Azure Service Bus** . Klik op **installeren** om de installatie te voltooien en sluit vervolgens het volgende dialoogvenster:

    ![][7]

U bent nu gereed om code te schrijven voor een Service Bus.

## <a name="create-a-service-bus-connection-string"></a>Een Service Bus verbindingsreeks maken

Een verbindingsreeks Service Bus gebruikt voor het opslaan van eindpunten en referenties. U kunt de verbindingsreeks in een configuratiebestand in plaats van hard te coderen deze plaatsen:

- Wanneer u Azure services gebruikt, verdient het aanbeveling de verbindingsreeks met het configuratiesysteem Azure service (.csdef en .cscfg-bestanden) op te slaan.
- Wanneer u websites Azure of Azure virtuele Machines, moet u de verbindingsreeks met de .NET configuratiesysteem (bijvoorbeeld het bestand Web.config) op te slaan.

In beide gevallen kunt u ophalen de connection string met behulp van de `CloudConfigurationManager.GetSetting` methode, zoals verderop in dit artikel wordt weergegeven.

### <a name="configure-your-connection-string"></a>De verbindingsreeks te configureren

Het mechanisme van service-configuratie kunt u configuratie-instellingen van het [portal Azure][] dynamisch te wijzigen zonder de toepassing opnieuw te implementeren. Bijvoorbeeld een `Setting` etiket en het bestand met de formulierdefinitie (**.csdef**) service, zoals in het volgende voorbeeld wordt getoond.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

U kunt vervolgens waarden opgeven in het service-configuratiebestand (.cscfg).

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Gebruik de sleutelnaam gedeelde toegang handtekening (SAS) en de waarden opgehaald van de portal, zoals eerder is beschreven.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>De verbindingsreeks configureren wanneer u websites Azure of Azure virtuele Machines

Wanneer u websites of virtuele Machines, verdient het aanbeveling het configuratiesysteem .NET (bijvoorbeeld Web.config) te gebruiken. Slaat u de connection string met behulp van de `<appSettings>` element.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

De SAS en de waarden die u opgehaald uit de [Azure portal][]gebruiken zoals hierboven beschreven.

## <a name="create-a-topic"></a>Een onderwerp maken

U kunt beheertaken uit te voeren voor een Service Bus onderwerpen en abonnementen met behulp van de klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) kunt uitvoeren. Deze klasse bevat methoden voor het maken, opsommen en verwijderen van onderwerpen.

In het volgende voorbeeld wordt een `NamespaceManager` de Azure-object `CloudConfigurationManager` klasse met een verbindingsreeks die bestaat uit het basisadres van een naamruimte Bus-Service en de juiste SAS met machtigingen voor het beheren van deze referenties. Deze verbindingsreeks heeft de volgende vorm:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Gebruik het volgende voorbeeld wordt gegeven van de configuratie-instellingen in de vorige sectie.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Er zijn overbelastingen van de [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) -methode waarmee u de eigenschappen van het onderwerp; bijvoorbeeld als u wilt instellen van de standaard waarde time-to-live (TTL) die worden toegepast op berichten die worden verzonden naar het onderwerp. Deze instellingen worden toegepast met behulp van de klasse [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) . In het volgende voorbeeld ziet u hoe een onderwerp met de naam **TestTopic** met een maximale grootte van 5 GB en een standaardbericht TTL van 1 minuut te maken.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] U kunt de methode [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) op [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -objecten gecontroleerd of een onderwerp met een opgegeven naam al bestaat in een naamruimte.

## <a name="create-a-subscription"></a>Een abonnement maken

Ook kunt u met behulp van de klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) onderwerp-abonnementen. Abonnementen worden benoemd en een optionele filter dat ervoor zorgt dat de reeks berichten die worden doorgegeven aan de virtuele wachtrij van het abonnement kunnen hebben.

> [AZURE.IMPORTANT] Om berichten te ontvangen met een abonnement, moet u dit abonnement vóór het verzenden van berichten naar het onderwerp. Als er geen abonnementen op een onderwerp, het onderwerp, worden die berichten.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement met het standaard (MatchAll)-filter maken

Als er geen filter is opgegeven bij het maken van een nieuw abonnement, is het **MatchAll** -filter standaard wordt dit filter die wordt gebruikt. Wanneer u het filter **MatchAll** gebruikt, worden alle berichten die zijn gepubliceerd op het onderwerp in het abonnement van virtuele wachtrij geplaatst. In het volgende voorbeeld maakt u een abonnement met de naam 'AllMessages' en het standaardfilter **MatchAll** gebruikt.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

U kunt ook filters waarmee u kunt opgeven welke berichten worden verzonden naar een onderwerp moeten instellen binnen een bepaald onderwerp abonnement weergegeven.

De meest flexibele type filter wordt ondersteund door abonnementen is de klasse [SqlFilter][] , die een subset van de SQL92-standaard implementeert. SQL-filters worden toegepast op de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Zie voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter, de syntaxis [SqlFilter.SqlExpression][] .

In het volgende voorbeeld maakt u een abonnement met de naam **HighMessages** met een [SqlFilter][] -object dat u selecteert alleen berichten met een aangepaste eigenschap op de **MessageNumber** groter is dan 3.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Het volgende voorbeeld wordt op dezelfde manier een abonnement met de naam **LowMessages** met een [SqlFilter][] dat alleen berichten met een **MessageNumber** eigenschap kleiner is dan of gelijk aan 3 selecteert.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Nu wanneer een bericht wordt verzonden naar `TestTopic`, het wordt altijd geleverd met een abonnement op het onderwerp **AllMessages** abonnement en selectief wordt geleverd met een abonnement op de **HighMessages** en **LowMessages** onderwerp abonnementen (afhankelijk van de inhoud van het bericht)-ontvangers-ontvangers.

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp

De toepassing maakt u een bericht verzendt naar een onderwerp Service Bus, een [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) -object met de verbindingsreeks.

De volgende code toont het maken van een [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) -object voor het **TestTopic** onderwerp gemaakt eerder met behulp van de [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) API-aanroep.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Berichten die worden verzonden naar Service Bus onderwerpen zijn instanties van de klasse [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) -objecten hebben een aantal standaard eigenschappen (zoals [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) en [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), een woordenlijst die wordt gebruikt om aangepaste toepassing-specifieke eigenschappen en een willekeurige toepassing gegevens. Een toepassing kunt instellen in de hoofdtekst van het bericht een geserialiseerd object wordt doorgegeven aan de constructor van het object [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) en de juiste **DataContractSerializer** wordt vervolgens gebruikt voor het serialiseren van het object. U kunt ook kan een **System.IO.Stream** worden geleverd.

In het volgende voorbeeld laat zien hoe vijf testberichten verzenden naar het **TestTopic** [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) object verkregen in het vorige voorbeeld. Opmerking de waarde van de eigenschap [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) van elk bericht is afhankelijk van de herhaling van de lus (Hiermee wordt bepaald welke abonnementen ontvangen).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Onderwerpen Service Bus ondersteunt een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [premie laag](service-bus-premium-messaging.md). De kop, die de van standaard- en aangepaste toepassingseigenschappen bevat, hebben een maximale grootte van 64 KB. Er is geen limiet op het aantal berichten in een onderwerp gehouden, maar er is een cap op de totale grootte van de berichten die door een onderwerp. De grootte van dit onderwerp is gedefinieerd bij het maken, met een maximum van 5 GB. Als partitioneren is ingeschakeld, is de bovengrens hoger. Zie [Partitioned entiteiten messaging](service-bus-partitioning.md)voor meer informatie.

## <a name="how-to-receive-messages-from-a-subscription"></a>Het ontvangen van berichten van een abonnement

De aanbevolen manier om berichten te ontvangen van een abonnement is een [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) -object te gebruiken. [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) -objecten kunnen in twee verschillende modi werken: [ *ReceiveAndDelete* en *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Wanneer de modus **ReceiveAndDelete** wordt is een single-shot-bewerking dat wil zeggen, wanneer de Bus Service een leesbevestiging voor een bericht in een abonnement ontvangt, het markeert het bericht worden gebruikt en terug naar de toepassing. **ReceiveAndDelete** modus is de eenvoudigste en meest geschikt voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat het bericht is gemarkeerd als Service Bus als verbruikt wanneer de toepassing wordt opnieuw opgestart en begint het opnieuw gebruiken van berichten, beschikt het hebt gemist het bericht dat is verbruikt vóór de crash.

In de modus **PeekLock** (dit is de standaardmodus) verandert in de procedure ontvangen een bewerking met twee fasen, die het mogelijk maakt voor van ondersteuningstoepassingen die ontbrekende berichten kunnen niet tolereren. Bus-Service een aanvraag ontvangt, deze vindt u het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen als geeft als resultaat de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), wordt de tweede fase van de procedure ontvangen door het aanroepen van [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) voor het ontvangen bericht voltooid. Wanneer Service Bus **volledig** bellen, markeert u het bericht worden gebruikt en verwijderd uit het abonnement.

In het volgende voorbeeld wordt gedemonstreerd hoe berichten kunnen worden ontvangen en verwerkt met behulp van de standaard **PeekLock** -modus. Als u een andere waarde voor [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) , kunt u een andere overbelasting voor [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx). In dit voorbeeld wordt de callback [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) op berichten die aankomen in het **HighMessages** -abonnement.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

In dit voorbeeld wordt de [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) -retouraanroep met behulp van een [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) -object geconfigureerd. [AutoAanvullen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) is ingesteld op **false** om handmatige besturing van de [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) aanroepen voor het ontvangen bericht. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) is ingesteld op 1 minuut, waardoor de client wacht maximaal één minuut voordat u de functie Automatische verlenging wordt beëindigd en maakt de client een nieuwe oproep om berichten te controleren. De waarde van deze eigenschap vermindert het aantal keren dat de client worden toerekenbare aanroepen die berichten kunnen niet worden opgehaald.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het afhandelen van toepassing crashes en berichten onleesbaar

Bus service biedt functies waarmee u zonder problemen herstellen van fouten in uw toepassing of moeilijkheden verwerken van een bericht. Als een ontvangende toepassing het bericht verwerkt voor een of andere reden niet kan, kan deze de methode [Abandon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) aanroepen voor het ontvangen bericht (in plaats van de methode [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ). Dit zorgt ervoor dat de Bus-Service het bericht binnen het abonnement te ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen door de toepassing in beslag nemen, of door een andere toepassing van de in beslag.

Er is ook een time-outwaarde die is gekoppeld aan een bericht in het abonnement is vergrendeld en als de aanvraag niet verwerken het bericht voordat de time-out vergrendeling verloopt (bijvoorbeeld, als de toepassing loopt vast), en vervolgens de Bus-Service het bericht automatisch wordt ontgrendeld en beschikbaar moeten opnieuw worden ontvangen.

In het geval dat de toepassing vastloopt nadat het bericht is verwerkt, maar voordat de aanvraag [volledig](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) is ingediend, wordt het bericht geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd *tenminste eenmaal worden verwerkt*; elk bericht wordt verwerkt, ten minste één keer maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan niet dubbele verwerking tolereren, kunnen ontwikkelaars van toepassingen moeten extra logica toevoegen aan hun toepassing voor aflevering van dubbele berichten verwerken. Dit wordt vaak bereikt met behulp van de eigenschap [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) van het bericht over zelf constant blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

In het volgende voorbeeld laat zien hoe het onderwerp **TestTopic** verwijderen uit de naamruimte **HowToSample** service.

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Als u een onderwerp verwijdert verwijderd alle abonnementen die zijn geregistreerd bij het onderwerp ook. Abonnementen kunnen ook afzonderlijk worden verwijderd. De volgende code laat zien hoe een abonnement met de naam **HighMessages** in het onderwerp **TestTopic** worden verwijderd.

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de grondbeginselen van Service Bus onderwerpen en abonnementen, klik op deze koppelingen voor meer informatie.

-   [Wachtrijen, onderwerpen, en abonnementen][].
-   [Onderwerp filters monster][]
-   API-Naslaggids voor [SqlFilter][].
-   Bouwen van een werkende toepassing verzenden en ontvangen van berichten en een wachtrij Service Bus: [Bus Service brokered messaging .NET zelfstudie][].
-   Service Bus monsters: downloaden van [Azure monsters][] of [Overzicht](service-bus-samples.md).

  [Azure portal]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
  [Onderwerp filters monster]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Zelfstudie voor messaging .NET Service Bus brokered]: service-bus-brokered-tutorial-dotnet.md
  [Azure monsters]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
