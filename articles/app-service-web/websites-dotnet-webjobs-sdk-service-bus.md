<properties 
    pageTitle="Het gebruik van Azure Service Bus met de WebJobs SDK" 
    description="Informatie over het gebruik van wachtrijen Azure Service Bus en onderwerpen met de WebJobs SDK." 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Het gebruik van Azure Service Bus met de WebJobs SDK

## <a name="overview"></a>Overzicht

Deze handleiding bevat C# codevoorbeelden die hoe een proces wordt geactiveerd tonen wanneer een Azure Service Bus is ontvangen. De codevoorbeelden gebruiken [WebJobs SDK](websites-dotnet-webjobs-sdk.md) versie 1.x.

De handleiding wordt ervan uitgegaan dat u weet [hoe u een WebJob-project in Visual Studio met tekenreeksen die naar uw account opslag verwijzen maken](websites-dotnet-webjobs-sdk-get-started.md).

De codefragmenten weergeven alleen functies, niet de code waarmee de `JobHost` object, zoals in dit voorbeeld:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

Een [complete Service Bus voorbeeld](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) is in de bibliotheek azure-webjobs-sdk-voorbeelden op GitHub.com.

## <a id="prerequisites"></a>Vereisten

U hebt het pakket [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet naast de WebJobs SDK-pakketten te installeren als u wilt werken met Service Bus. 

U hebt ook de verbindingsreeks AzureWebJobsServiceBus naast de opslag verbindingsreeksen ingesteld.  U kunt dit doen het `connectionStrings` sectie van het bestand App.config, zoals in het volgende voorbeeld wordt getoond:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Zie voor een voorbeeldproject waarin de instelling voor de tekenreeks Service Bus in het bestand App.config [voorbeeld Service Bus](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

De verbindingsreeks kunnen ook worden ingesteld in de Azure runtime-omgeving, die vervolgens de instellingen App.config overschrijft wanneer de WebJob wordt uitgevoerd in Azure; Zie [Aan de slag met de WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account)voor meer informatie.

## <a id="trigger"></a>Hoe een functie wordt geactiveerd wanneer een wachtrij Service Bus bericht wordt ontvangen

Als u wilt een functie schrijven die de WebJobs SDK wordt aangeroepen wanneer een wachtrij-bericht wordt ontvangen, gebruikt u de `ServiceBusTrigger` kenmerk. De constructor kenmerk heeft een parameter met de naam van de wachtrij op te vragen.

### <a name="how-servicebustrigger-works"></a>De werking van ServiceBusTrigger

De SDK ontvangt een bericht in `PeekLock` -modus en gesprekken `Complete` in het bericht als de functie is voltooid of oproepen `Abandon` als de functie is mislukt. Als de functie wordt uitgevoerd langer dan de `PeekLock` -time-out van de vergrendeling automatisch wordt vernieuwd.

Bus service omgaat met haar eigen poison wachtrij die niet kan worden beheerd of geconfigureerd door de WebJobs SDK. 

### <a name="string-queue-message"></a>String wachtrij bericht

In het volgende voorbeeld leest een wachtrij-bericht bevat een tekenreeks op en schrijft u de tekenreeks voor het dashboard WebJobs SDK.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Opmerking:** Als u de berichten in wachtrij plaatsen in een toepassing die de WebJobs SDK maakt geen gebruik van maakt, moet u [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) instellen op "text/plain".

### <a name="poco-queue-message"></a>POCO wachtrij bericht

De SDK wordt automatisch een bericht wachtrij met een POCO JSON deserialize [(gewone oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) type. In het volgende voorbeeld wordt een bericht wachtrij met een `BlobInformation` object met een `BlobName` eigenschap:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Zie voor voorbeelden van programmacode waarin wordt getoond hoe de eigenschappen van de POCO gebruiken om te werken met tabellen in dezelfde functie en blobs, de [opslag wachtrijen versie van dit artikel](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Als de code die het bericht wachtrij maakt geen gebruik van de WebJobs SDK, gebruikt u code die er ongeveer als volgt uitzien:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Typen ServiceBusTrigger werkt met

Naast `string` en POCO typen, kunt u de `ServiceBusTrigger` -kenmerk met een matrix van bytes of een `BrokeredMessage` object.

## <a id="create"></a>Het maken van Service Bus berichten in wachtrij plaatsen

Om te schrijven een functie die een nieuwe wachtrij bericht gebruik maakt de `ServiceBus` kenmerk- en in naam van de wachtrij aan de constructor van het kenmerk. 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Een bericht één wachtrij maken in een niet-async-functie

In het volgende voorbeeld wordt een uitvoerparameter maken een nieuw bericht in de wachtrij met de naam 'outputqueue' met dezelfde inhoud als het bericht is ontvangen in de wachtrij met de naam 'inputqueue'.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

De uitvoerparameter voor het maken van een enkele wachtrij kan een van de volgende typen zijn:

* `string`
* `byte[]`
* `BrokeredMessage`
* Een geserialiseerd POCO type die u definieert. Automatisch geserialiseerd als JSON.

POCO type parameters, wordt een bericht wachtrij altijd gemaakt wanneer de functie eindigt; Als de parameter null is, maakt de SDK een bericht wachtrij null als resultaat geven wordt wanneer het bericht is ontvangen en gedeserialiseerd. Voor andere typen als de parameter null is wordt geen bericht wachtrij gemaakt.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Maak meerdere berichten in wachtrij plaatsen of in de asynchrone functies

U kunt meerdere berichten maken de `ServiceBus` met het kenmerk `ICollector<T>` of `IAsyncCollector<T>`, zoals in het volgende codevoorbeeld:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Elk bericht wachtrij wordt onmiddellijk gemaakt wanneer de `Add` methode wordt aangeroepen.

## <a id="topics"></a>Werken met onderwerpen Service Bus

Als een functie die wordt aangeroepen wanneer een bericht wordt ontvangen op een onderwerp Service Bus de SDK wilt opslaan, gebruikt u de `ServiceBusTrigger` -kenmerk met de constructor wordt de onderwerpnaam naam van abonnement, zoals in het volgende codevoorbeeld:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

U kunt een bericht over een onderwerp maken de `ServiceBus` -kenmerk met de naam van een onderwerp hetzelfde gebruik in combinatie met de naam van een wachtrij.

## <a name="features-added-in-release-11"></a>Functies die zijn toegevoegd in versie 1.1

De volgende functies zijn toegevoegd in versie 1.1:

* Kunnen diep-berichten `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`aanpassing van de Bus-Service ondersteunt `MessagingFactory` en `NamespaceManager`.
* A `MessageProcessor` strategiepatroon kunt u een processor per wachtrij/onderwerp opgeven.
* Bericht verwerking gelijktijdigheid wordt standaard ondersteund. 
* Eenvoudige aanpassing van de `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.
* Toestaan dat [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) moet worden opgegeven in `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (voor scenario's waarin u mogelijk geen rechten beheren). 

## <a id="queues"></a>Verwante onderwerpen waarop de opslag wachtrijen artikel

Zie voor meer informatie over de SDK WebJobs scenario's die geen specifieke Service Bus [Azure wachtrij opslag met de WebJobs SDK gebruiken](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Onderwerpen in dit artikel zijn de volgende:

* Async-functies
* Meerdere exemplaren
* Systeem correct wordt afgesloten
* Kenmerken WebJobs SDK in de hoofdtekst van een functie te gebruiken
* De SDK-verbindingsreeksen instellen in code
* Waarden instellen voor WebJobs SDK constructor parameters in de code
* Handmatig een functie activeren
* Schrijven van Logboeken

## <a id="nextsteps"></a>Volgende stappen

Deze handleiding heeft verstrekt codevoorbeelden waarin wordt aangegeven hoe u veelvoorkomende scenario's voor het werken met Azure Service Bus te verwerken. Zie voor meer informatie over het gebruik van Azure WebJobs en de WebJobs SDK [Azure WebJobs aanbevolen bronnen](http://go.microsoft.com/fwlink/?linkid=390226).
 
