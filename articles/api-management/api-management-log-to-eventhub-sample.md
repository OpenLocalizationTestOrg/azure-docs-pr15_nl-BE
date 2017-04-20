<properties
    pageTitle="De API's met Azure API Management, gebeurtenis Hubs en Runscope controleren"
    description="Voorbeeldtoepassing om aan te tonen in het logboek-eventhub-beleid verbinding Azure API Management, Azure gebeurtenis Hubs en Runscope voor HTTP vastleggen en bewaken"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>De API's met Azure API Management, gebeurtenis Hubs en Runscope controleren

De [API-Management-service](api-management-key-concepts.md) biedt veel mogelijkheden voor het verbeteren van de verwerking van HTTP-aanvragen die zijn verzonden naar uw HTTP-API. Het bestaan van de aanvragen en antwoorden zijn echter tijdelijk. Het verzoek is ingediend en deze loopt via de API-Management-service om uw back-end API. De API verwerkt de aanvraag en een antwoord doorloopt aan de consument API. De API-Management-service houdt enkele belangrijke statistische gegevens over de API's voor de weergave in het dashboard van de Publisher-portal, maar ook buiten, de details verdwenen zijn.

U kunt de details van de aanvraag en het antwoord op een [Gebeurtenis-Hub Azure](../event-hubs/event-hubs-what-is-event-hubs.md)verzenden met behulp van het [logboek naar eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [beleid](api-management-howto-policies.md) in de API-Management-service. Er zijn verschillende redenen waarom u mogelijk gebeurtenissen moet genereren van HTTP-berichten die worden verzonden naar de API's. Enkele voorbeelden zijn de audittrail van updates, gebruik van analytics, uitzondering signalering en 3de partij integraties.   

In dit artikel laat zien hoe de hele HTTP-aanvraag en het antwoord bericht vastleggen, verzenden naar een gebeurtenis Hub en vervolgens sturen dat bericht aan een derde partij service voor HTTP vastleggen en bewaken van services.

## <a name="why-send-from-api-management-service"></a>Waarom verzenden van de Service Management API?
Het is mogelijk voor het schrijven van HTTP-middleware die HTTP API frameworks voor het vastleggen van HTTP-aanvragen en antwoorden en deze feed in de registratie- en bewakingssystemen kunt aansluiten. Het nadeel van deze aanpak is de HTTP-middleware moet worden geïntegreerd in de backend-API en moet overeenkomen met het platform van de API. Als er meerdere API's moet de middleware implementeren door elkaar. Vaak zijn de redenen waarom de backend-API's kan niet worden bijgewerkt.

De Azure API Management-service gebruiken voor integratie met de infrastructuur voor logboekregistratie biedt een centrale en platform-onafhankelijke-oplossing. Het is ook schaalbare, deels als gevolg van de [geo-replicatie](api-management-howto-deploy-multi-region.md) mogelijkheden van Azure API beheer.

## <a name="why-send-to-an-azure-event-hub"></a>Waarom verzenden op een gebeurtenis Azure Hub?
Het is een redelijk te vragen, waarom maakt u een beleid dat specifiek is voor Azure gebeurtenis Hubs? Er zijn vele verschillende plaatsen waar ik eventueel mijn aanvragen moeten worden geregistreerd. Waarom niet gewoon versturen van aanvragen rechtstreeks naar de eindbestemming?  Dat is een optie. Wanneer u logboekregistratie van aanvragen van een API-service management, is het echter noodzakelijk te overwegen hoe de prestaties van de API is van invloed op logboekberichten. Geleidelijke stijgingen van de belasting kunnen worden verwerkt door de beschikbare exemplaren van onderdelen van het systeem te verhogen of door gebruik te maken van de geo-replicatie. Korte pieken in het verkeer veroorzaakt echter verzoeken aanzienlijk worden vertraagd als aanvragen voor registratie infrastructuur onder belasting langzamer te starten.

De gebeurtenis Azure Hubs is ontworpen voor ingress enorme hoeveelheden gegevens, met een capaciteit voor het omgaan met een aantal gebeurtenissen die veel hoger dan het aantal HTTP-verwerken voor de meeste API's aanvragen. De gebeurtenis Hub fungeert als een soort geavanceerde buffer tussen uw API management-service en de infrastructuur die de berichten verwerken en opslaan. Dit zorgt ervoor dat de prestaties van de API niet als gevolg van de infrastructuur voor logboekregistratie afnemen.  

Zodra de gegevens zijn doorgegeven aan een gebeurtenis Hub wordt behouden en zal wachten op gebeurtenis Hub consumenten verwerken. De gebeurtenis Hub maakt dit niets uit hoe deze worden verwerkt, het is net belangrijk ervoor te zorgen dat het bericht is bezorgd.     

Gebeurtenis Hubs hebben de mogelijkheid om de stroom van gebeurtenissen aan meerdere groepen van consumenten. Hierdoor kunnen gebeurtenissen worden verwerkt door de totaal verschillende systemen. Hierdoor ondersteunt vele scenario's voor integratie zonder toevoeging vertragingen in de verwerking van de aanvraag van de API binnen de service Management API plaatsen als slechts één gebeurtenis moet worden gegenereerd.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Een beleid voor toepassing/HTTP-berichten sturen
Een gebeurtenis Hub accepteert gegevens als een eenvoudige tekenreeks. De inhoud van de reeks is volledig aan u. Om te kunnen inpakken van een HTTP-aanvraag en naar uitschakelen gebeurtenis Hubs moeten we de tekenreeks met de gegevens van de aanvraag of -antwoord. In dit soort situaties, als er een bestaande opmaak kan worden hergebruikt en we niet moeten schrijven onze eigen code te parseren. In eerste instantie als ik met de [HAR](http://www.softwareishard.com/blog/har-12-spec/) voor het verzenden van HTTP-aanvragen en antwoorden. Deze indeling is echter geoptimaliseerd voor een reeks van HTTP-aanvragen op te slaan in een indeling op basis van JSON. Bevat een aantal verplichte elementen die onnodige complexiteit voor het scenario van het HTTP-bericht doorgegeven via het netwerk wordt toegevoegd.  

Een alternatief is gebruik van de `application/http` media type als beschreven in de HTTP-specificatie [RFC 7230](http://tools.ietf.org/html/rfc7230). Dit mediatype gebruikt exact dezelfde indeling die daadwerkelijk HTTP-berichten sturen via het netwerk wordt gebruikt, maar het hele bericht kan worden geplaatst in de hoofdtekst van een andere HTTP-aanvraag. In ons geval gaan we gewoon het lichaam als ons bericht gebruiken voor het verzenden van de gebeurtenis Hubs. Gemakkelijk, is er een parser die bestaat in de bibliotheken van [Microsoft ASP.NET Web API 2.2-Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) die kunnen deze indeling analyseren en omzetten in de native `HttpRequestMessage` en `HttpResponseMessage` objecten.

Op basis van [beleid expressies](https://msdn.microsoft.com/library/azure/dn910913.aspx) in Azure API Management te kunnen maken van dit bericht dat we nodig hebben om te profiteren van C#. Hier is het beleid dat een http-request-bericht naar Azure gebeurtenis Hubs verzendt.

       <log-to-eventhub logger-id="conferencelogger" partition-id="0">
       @{
           var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                       context.Request.Method,
                                                       context.Request.Url.Path + context.Request.Url.QueryString);

           var body = context.Request.Body?.As<string>(true);
           if (body != null && body.Length > 1024)
           {
               body = body.Substring(0, 1024);
           }

           var headers = context.Request.Headers
                                  .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                  .ToArray<string>();

           var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

           return "request:"   + context.Variables["message-id"] + "\n"
                               + requestLine + headerString + "\r\n" + body;
       }
       </log-to-eventhub>

### <a name="policy-declaration"></a>Verklaring van beleid
Er een paar specifieke dingen opgemerkt over de beleidsexpressie van dit. Het logboek op eventhub beleid is een kenmerk met de naam logboek-id die verwijst naar de naam van het logboek dat is gemaakt in de API-Management-service. De details van een gebeurtenis Hub logger in de API-Management-service instellen vindt u in het document [het registreren van gebeurtenissen met Azure gebeurtenis Hubs in Azure API beheer](api-management-howto-log-event-hubs.md). Het tweede kenmerk is een optionele parameter waarmee u gebeurtenis Hubs die partitie voor het opslaan van het bericht in. Gebeurtenis Hubs partities gebruiken om te schakelen van schaalbaarheid en vereist een minimum van twee. De aflevering van berichten is alleen gegarandeerd in een partitie. Als we de opdracht gebeurtenis Hub op welke partitie het bericht plaatsen wordt gebruikt een algoritme round robin verdelen het niet geven. Dat kan echter dat sommige van onze berichten op volgorde worden verwerkt.  

### <a name="partitions"></a>Partities
Ik wilde onze berichten aan de consument in volgorde worden afgeleverd en profiteren van de mogelijkheid van laden verdeling van partities, zodat http-request-berichten verzenden naar één partitie en HTTP-reactie-berichten op een tweede partitie. Zo ziet een gelijkmatige belasting verdeling en wij kunnen garanderen dat alle aanvragen worden in volgorde worden verbruikt en alle antwoorden op volgorde wordt verbruikt. Het is mogelijk een antwoord te worden gebruikt voor het desbetreffende verzoek, maar is dat geen probleem, zoals we hebben een ander mechanisme voor het correleren van aanvragen om te antwoorden en we weten dat aanvragen altijd voorafgaan antwoorden.

### <a name="http-payloads"></a>HTTP-nettoladingen
Na het gebouw de `requestLine` we controleren om te zien als het hoofdgedeelte van de aanvraag moet worden afgekapt. Het hoofdgedeelte van de aanvraag wordt afgekapt tot 1024 alleen. Dit kan worden verhoogd, maar afzonderlijke berichten met gebeurtenis-Hub beperkt tot 256KB zijn, dus het is waarschijnlijk dat bepaalde HTTP-bericht wordt niet passen in een enkel bericht organen. Bij het uitvoeren van logboekregistratie en analytics kan een aanzienlijke hoeveelheid gegevens worden afgeleid van alleen de verzoekregel HTTP-headers en. Ook veel API aanvragen retourneert alleen kleine organisaties en het verlies van de waarde van de informatie door te breken grote organen is dus relatief ten opzichte van de vermindering van de overdracht, verwerking en opslag te houden van de inhoud van alle minimale. Een laatste opmerking over de verwerking van de instantie is dat we moeten geven `true` aan de<string>()-methode omdat we bij het lezen van de inhoud, maar is ook de backend-API te kunnen lezen van de tekst wilt. Door doorgeven waar deze methode hebben we het lichaam de buffer worden opgeslagen zodat het een tweede keer kan worden gelezen. Dit is belangrijk te weten als u beschikt over een API die van zeer grote bestanden uploaden of lange polling wordt gebruikt. In deze gevallen zou worden aanbevolen om te voorkomen dat het lichaam helemaal lezen.   

### <a name="http-headers"></a>HTTP-headers
HTTP-Headers kunnen alleen worden overgedragen via naar de berichtindeling in een indeling die eenvoudig sleutel/waarde-paar. We hebt het verwijderen van bepaalde gevoelige velden van beveiliging om te voorkomen dat onnodig lekkende referentiegegevens gekozen. Het is onwaarschijnlijk dat API-sleutels en andere referenties voor analytics doeleinden zou worden gebruikt. Als we willen de analyse van de gebruiker en het product gebruiken en vervolgens krijgen we kunnen die uit de `context` object en die aan het bericht toevoegen.     
### <a name="message-metadata"></a>De metagegevens van het bericht
Als u het volledige bericht te verzenden naar de hub gebeurtenis, de eerste regel is niet echt deel uitmaakt van de `application/http` bericht. De eerste regel is extra metagegevens die bestaan uit of het bericht is een verzoek of response-bericht en een bericht-id die wordt gebruikt voor het correleren van aanvragen om te antwoorden. De bericht-id is gemaakt met een ander beleid, dat er als volgt uit:

    <set-variable name="message-id" value="@(Guid.NewGuid())" />

We kunnen het aanvraagbericht gemaakt, die in een variabele opgeslagen totdat het antwoord is geretourneerd en vervolgens gewoon de aanvraag en het antwoord als een enkel bericht verzonden. Echter door het verzenden van de aanvraag en het antwoord onafhankelijk van elkaar en een bericht-id voor het correleren van de twee, krijgen we een beetje meer flexibiliteit in de grootte van het bericht, de mogelijkheid om te profiteren van meerdere partities waarbij de handhaving van de volgorde van berichten en de aanvraag wordt eerder worden weergegeven in onze dashboard logboekregistratie. Ook kunnen er bepaalde scenario's waar nooit een geldig antwoord wordt verzonden naar de hub gebeurtenis, mogelijk door een verzoek fatale fout in de service Management API, maar we zullen nog steeds een record van de aanvraag.

Het beleid voor het verzenden van het bericht van de HTTP-reactie lijkt erg op de aanvraag en dus de volledige beleidsconfiguratie uitziet:

      <policies>
        <inbound>
            <set-variable name="message-id" value="@(Guid.NewGuid())" />
            <log-to-eventhub logger-id="conferencelogger" partition-id="0">
              @{
                  var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                              context.Request.Method,
                                                              context.Request.Url.Path + context.Request.Url.QueryString);

                  var body = context.Request.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Request.Headers
                                       .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                       .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                       .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "request:"   + context.Variables["message-id"] + "\n"
                                      + requestLine + headerString + "\r\n" + body;
              }
          </log-to-eventhub>
        </inbound>
        <backend>
            <forward-request follow-redirects="true" />
        </backend>
        <outbound>
            <log-to-eventhub logger-id="conferencelogger" partition-id="1">
              @{
                  var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                                      context.Response.StatusCode,
                                                      context.Response.StatusReason);

                  var body = context.Response.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Response.Headers
                                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                                  .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "response:"  + context.Variables["message-id"] + "\n"
                                      + statusLine + headerString + "\r\n" + body;
             }
          </log-to-eventhub>
        </outbound>
      </policies>

De `set-variable` beleid maakt een waarde die toegankelijk is voor zowel de `log-to-eventhub` beleid in de `<inbound>` sectie en de `<outbound>` sectie.  

## <a name="receiving-events-from-event-hubs"></a>Ontvangen van gebeurtenissen van gebeurtenis Hubs
Gebeurtenissen van Azure gebeurtenis Hub worden ontvangen via het [protocol van AMQP](http://www.amqp.org/). Het team van Microsoft Service Bus aangebrachte client bibliotheken beschikbaar zijn voor het vereenvoudigen van de gebeurtenissen in beslag nemen. Er zijn twee verschillende benaderingen worden ondersteund, wordt een *Directe consument* en de andere de `EventProcessorHost` klasse. In de [Gebeurtenis Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md)vindt u voorbeelden van deze twee benaderingen. De korte versie van de verschillen is, `Direct Consumer` hebt u volledige controle en de `EventProcessorHost` , worden enkele van de Loodgieterswerk voor u maar kunt u bepaalde veronderstellingen over hoe u deze gebeurtenissen worden verwerkt.  

### <a name="eventprocessorhost"></a>EventProcessorHost
In dit voorbeeld gebruiken we de `EventProcessorHost` voor eenvoud, maar het kan niet de beste keuze voor dit scenario. `EventProcessorHost`het harde werk van en zorg dat u geen zorgen te maken over problemen binnen een bepaalde gebeurtenis Processorklasse threading biedt. In ons scenario, zijn we echter gewoon het bericht naar een andere indeling converteren en deze langs een andere service met behulp van een asynchrone methode wordt doorgegeven. Er is niet nodig voor het bijwerken van de gedeelde status en daarom geen risico van threading problemen. Voor de meeste scenario's, `EventProcessorHost` is waarschijnlijk de beste keuze en het is zeker gemakkelijker optie.     

### <a name="ieventprocessor"></a>IEventProcessor
Het centrale concept bij het gebruik van `EventProcessorHost` is voor het maken van een implementatie van de `IEventProcessor` interface met de methode `ProcessEventAsync`. De essentie van deze methode wordt hier weergegeven:

  asynchrone taak IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages) {

           foreach (EventData eventData in messages)
           {
               _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

               try
               {
                   var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
                   await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
               }
               catch (Exception ex)
               {
                   _Logger.LogError(ex.Message);
               }
           }
            ... checkpointing code snipped ...
        }

Een lijst met EventData objecten worden doorgegeven aan de methode en we die lijst doorlopen. Het aantal bytes van elke methode zijn verdeeld in een HttpMessage-object en dit object wordt doorgegeven aan een exemplaar van IHttpMessageProcessor.

### <a name="httpmessage"></a>HttpMessage
De `HttpMessage` exemplaar bevat drie typen gegevens:

      public class HttpMessage
       {
           public Guid MessageId { get; set; }
           public bool IsRequest { get; set; }
           public HttpRequestMessage HttpRequestMessage { get; set; }
           public HttpResponseMessage HttpResponseMessage { get; set; }

        ... parsing code snipped ...

      }

De `HttpMessage` -exemplaar bevat een `MessageId` GUID waarmee wij het HTTP-verzoek verbinden met de bijbehorende HTTP-antwoord en een boolean-waarde die aangeeft of het object een exemplaar van een HttpRequestMessage en HttpResponseMessage bevat. Met behulp van de ingebouwde in klassen van HTTP- `System.Net.Http`, kon ik gebruik te maken van de `application/http` bij het parseren van code die is opgenomen in `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
De `HttpMessage` exemplaar wordt vervolgens doorgestuurd naar de uitvoering van de `IHttpMessageProcessor` is een interface die ik heb gemaakt om de ontvangst en de interpretatie van de gebeurtenis van Azure gebeurtenis Hub en de effectieve verwerking van het loskoppelen.


## <a name="forwarding-the-http-message"></a>Het HTTP-bericht doorsturen
Voor dit voorbeeld besloten heb ik dat het is interessant om de HTTP-aanvraag naar [Runscope](http://www.runscope.com)via. Runscope is een cloud gebaseerde service die gespecialiseerd in HTTP is-fouten opsporen, het vastleggen en bewaken. Ze hebben een vrij laag, zodat is het gemakkelijk om te proberen en wij ons kunnen voor een overzicht van de HTTP-aanvragen in real-time door onze service Management API stroomt.

De `IHttpMessageProcessor` uitvoering uitziet,

      public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
       {
           private HttpClient _HttpClient;
           private ILogger _Logger;
           private string _BucketKey;
           public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
           {
               _HttpClient = httpClient;
               var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
               _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
               _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
               _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
               _Logger = logger;
           }

           public async Task ProcessHttpMessage(HttpMessage message)
           {
               var runscopeMessage = new RunscopeMessage()
               {
                   UniqueIdentifier = message.MessageId
               };

               if (message.IsRequest)
               {
                   _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
                   runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
               }
               else
               {
                   _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
                   runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
               }

               var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
               messagesLink.BucketKey = _BucketKey;
               messagesLink.RunscopeMessage = runscopeMessage;
               var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
               _Logger.LogDebug("Request sent to Runscope");
           }
       }

Ik kon profiteren van een [bestaande clientbibliotheek voor Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) die gemakkelijk push `HttpRequestMessage` en `HttpResponseMessage` exemplaren van in hun bedrijf. Toegang tot de API Runscope moet u een account en een API-sleutel. In de screencast [Toepassingen maken voor toegang tot Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) vindt u instructies voor het verkrijgen van een API-sleutel.

## <a name="complete-sample"></a>Volledige monster
De [broncode](https://github.com/darrelmiller/ApimEventProcessor) en tests voor de steekproef zijn op Github. U moet een [API Management-Service](api-management-get-started.md), [Een gekoppelde gebeurtenis Hub](api-management-howto-log-event-hubs.md)en een [Opslag Account](../storage/storage-create-storage-account.md) worden uitgevoerd van het monster.   

Het monster is gewoon een eenvoudige consoletoepassing die luistert naar gebeurtenissen die afkomstig zijn van de Hub van de gebeurtenis, worden geconverteerd naar een `HttpRequestMessage` en `HttpResponseMessage` objecten en stuurt deze door naar de Runscope-API.

In de volgende animatie ziet u een aanvraag naar een API in de Developer-Portal, de Console-programma waarin het bericht wordt ontvangen, verwerkt en doorgestuurd en vervolgens de aanvraag en het antwoord weergegeven in het itemvenster Runscope verkeer.

![Demonstratie van de aanvraag wordt doorgestuurd naar Runscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Samenvatting
Azure API Management-service biedt een ideale plaats voor het vastleggen van het HTTP-verkeer naar en van de API's op reis. Azure gebeurtenis Hubs is een zeer schaalbare, voordelige oplossing voor het vastleggen van dat verkeer en het voederen in verwerkingssystemen voor de van secundaire, bewaking en andere geavanceerde analytics. Verbinding maken met 3de partij verkeer monitoring systemen zoals Runscope een eenvoudig als een paar dozijn regels code is.

## <a name="next-steps"></a>Volgende stappen
-   Meer informatie over Azure gebeurtenis Hubs
    -   [Aan de slag met Azure gebeurtenis Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Ontvangen berichten met EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Gebeurtenis Hubs programming guide](../event-hubs/event-hubs-programming-guide.md)
-   Meer informatie over de integratie van de beheer-API en gebeurtenis Hubs
    -   [Het registreren van gebeurtenissen met Azure gebeurtenis Hubs in Azure API beheer](api-management-howto-log-event-hubs.md)
    -   [Verwijzing naar Logger entiteit](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [logboek voor eventhub beleidsverwijzing](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    