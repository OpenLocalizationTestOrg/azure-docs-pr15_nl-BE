<properties
   pageTitle="Optimaliseren van uw Azure-code in Visual Studio | Microsoft Azure"
   description="Lees over hoe Azure code optimalisatie tools in Visual Studio de code maken, betrouwbaardere en meer."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="optimizing-your-azure-code"></a>Optimaliseren van uw Code Azure

Wanneer u toepassingen die gebruikmaken van Microsoft Azure programmeren, zijn er sommige coding procedures worden aanbevolen om te voorkomen dat problemen met de app schaalbaarheid, gedrag en prestaties in een cloud-omgeving. Microsoft biedt een analyseprogramma Azure Code die herkent en identificeert verscheidene van deze problemen vaak opgetreden en kunt u deze kunt oplossen. U kunt het hulpprogramma van Visual Studio via NuGet downloaden.

## <a name="azure-code-analysis-rules"></a>Azure regels met Code analyse

Met het analysehulpprogramma Azure Code gebruikt de volgende regels automatisch uw Azure code markeren wanneer er bekende problemen met prestaties beïnvloeden. Gevonden problemen worden weergegeven als een waarschuwingen of Compilerfouten. Correcties of suggesties voor het oplossen van de waarschuwing of fout zijn vaak opgenomen in een pictogram van een gloeilamp.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Vermijd het gebruik van (standaard in-process) gebruikt

### <a name="id"></a>ID

AP0000

### <a name="description"></a>Beschrijving

Als u (de standaard in-process) gebruikt voor cloud-toepassingen gebruikt, kunt u de sessiestatus kunt verliezen.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Standaard is de sessiestatusmodus die is opgegeven in het bestand web.config in het proces. Ook als er geen vermelding in het configuratiebestand opgegeven, standaard de sessiestatus modus in-process. De verwerkingsmodus sessiestatus in geheugen opgeslagen op de webserver. Wanneer een exemplaar opnieuw wordt gestart of een nieuwe sessie wordt gebruikt voor taakverdeling of failover-ondersteuning, is niet de sessiestatus opgeslagen in het geheugen op de webserver opgeslagen. Deze situatie wordt voorkomen dat de toepassing van de wolk schaalbare wordt.

ASP.NET-sessiestatus ondersteunt verschillende andere opslagopties voor sessiestatusgegevens: InProc, StateServer, SQLServer, aangepast, en uit. Het verdient aanbeveling gebruik te maken van aangepaste modus voor gegevens op een externe Session State archief, zoals een [provider voor het bestand Vgx. Azure Session State](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Oplossing

Een aanbevolen oplossing is om de sessiestatus wordt opgeslagen in een cache beheerde dienst. Informatie over het [Azure Session State-provider voor het bestand Vgx.](http://go.microsoft.com/fwlink/?LinkId=401521) gebruiken voor het opslaan van de sessiestatus. Sessiestatus kunt u ook opslaan in andere plaatsen zodat de toepassing van de wolk schaalbaar is. Meer informatie over alternatieve Lees oplossingen [Session State-modi](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Methode Run moet async niet worden

### <a name="id"></a>ID

AP1000

### <a name="description"></a>Beschrijving

Asynchrone methoden (zoals [wachten](https://msdn.microsoft.com/library/hh156528.aspx)) buiten de methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) maken en vervolgens de asynchrone methoden aanroepen vanuit [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Het declareren van de asynchrone methode [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) zorgt ervoor dat de rol van de werknemer een lus opnieuw invoeren.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Asynchrone methoden in de methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) zorgt ervoor dat de service cloud-runtime recyclen van de rol van de werknemer. Bij het starten van de rol van een werknemer, alle uitvoering van het programma vinden plaats in de methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Afsluiten van de methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) zorgt ervoor dat de rol van de werknemer op te starten. Wanneer de werknemer rol runtime de asynchrone methode raakt, verzendt alle bewerkingen na de asynchrone methode en geeft als resultaat. Dit zorgt ervoor dat de rol van de werknemer van de methode [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) afsluiten en opnieuw starten. In de volgende iteratie van de uitvoering van de rol van werknemer raakt de asynchrone methode opnieuw en opnieuw is gestart, veroorzaakt door de rol van de werknemer te recyclen, opnieuw ook.

### <a name="solution"></a>Oplossing

Plaats alle asynchrone bewerkingen buiten de methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Vervolgens roept de methode asynchroon geherstructureerd uit binnen de methode [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) zoals RunAsync () .wait. Met het analysehulpprogramma Azure Code kunt u dit probleem kunt oplossen.

Het volgende codefragment wordt getoond hoe de codecorrectie voor dit probleem:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Service Bus gedeelde toegang handtekening verificatie gebruiken

### <a name="id"></a>ID

AP2000

### <a name="description"></a>Beschrijving

Gedeelde toegang handtekening (SAS) gebruiken voor verificatie. Access Control-Service (ACS) wordt voor de verificatie van service bus vervangen.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Voor een betere beveiliging is Azure Active Directory vervangen door een ACS-verificatie SAS-verificatie. Zie [Azure Active Directory is de toekomst van de ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) voor informatie over het overgangsschema.

### <a name="solution"></a>Oplossing

SAS-verificatie gebruiken in uw toepassingen. In het volgende voorbeeld ziet u hoe een bestaande SAS-token wordt gebruikt om een naamruimte van de bus service of entiteit.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Zie de volgende onderwerpen voor meer informatie.

- Zie voor een overzicht, [Gedeelde handtekening verificatie met Bus Service](https://msdn.microsoft.com/library/dn170477.aspx)

- [Het gebruik van verificatie van handtekening gedeeld met Bus Service](https://msdn.microsoft.com/library/dn205161.aspx)

- Zie [verificatie met behulp van gedeelde toegang handtekening (SAS) met serviceabonnementen Bus](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c) voor een voorbeeldproject

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Overweeg het gebruik van OnMessage methode om te voorkomen dat 'lus ontvangen'

### <a name="id"></a>ID

AP2002

### <a name="description"></a>Beschrijving

Om te voorkomen dat in een 'ontvangen lus' gaat, het aanroepen van de methode **OnMessage** is een betere oplossing voor het ontvangen van berichten dan het aanroepen van de methode **ontvangen** . Echter als moet u de methode **ontvangen** en u wachttijd voor een niet-standaard-server opgeeft, moet dat de server wachttijd is meer dan een minuut.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Als u **OnMessage**aanroept, begint de client een intern bericht pomp die voortdurend de wachtrij of abonnement controleert. Deze pomp bericht bevat een lus die problemen met een oproep om berichten te ontvangen. Als het aanroepen van een optreedt time-out, verleent het een nieuwe oproep. De time-outperiode wordt bepaald door de waarde van de eigenschap [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) van het [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)dat wordt gebruikt.

Het voordeel van het gebruik van **OnMessage** in vergelijking met de **ontvangen** is dat gebruikers niet handmatig controleren op berichten, uitzonderingen, meerdere berichten tegelijkertijd verwerken en de berichten te voltooien.

Als u **ontvangen oproepen** zonder de standaardwaarde te gebruiken, moet u de waarde *ServerWaitTime* is meer dan een minuut. Als u *ServerWaitTime* op meer dan één minuut voorkomt dat de server een time-out opgetreden voordat het bericht volledig is ontvangen.

### <a name="solution"></a>Oplossing

Zie de volgende codevoorbeelden voor het gebruik van aanbevolen. Zie de [Methode QueueClient.OnMessage (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)en de [QueueClient.Receive-methode (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx)voor meer informatie.

Zie het ontwerppatroon [Primer voor asynchrone berichtafhandeling](https://msdn.microsoft.com/library/dn589781.aspx)ter verbetering van de prestaties van de Azure messaging-infrastructuur.

Hier volgt een voorbeeld van het gebruik van **OnMessage** om berichten te ontvangen.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Hier volgt een voorbeeld van het **ontvangen** met de standaardwachttijd voor de server.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Hier volgt een voorbeeld van het **ontvangen** met een wachttijd van de server niet standaard.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Overweeg het gebruik van asynchrone methoden voor Service Bus

### <a name="id"></a>ID

AP2003

### <a name="description"></a>Beschrijving

Asynchrone Service Bus methoden gebruiken voor het verbeteren van prestaties brokered Messaging.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Met asynchrone methoden kunt programma gelijktijdigheid van toepassing omdat de hoofdthread voor het uitvoeren van ieder gesprek niet blokkeren. Wanneer u de Service Bus berichten methoden, uitvoeren van een bewerking (verzenden, ontvangen, verwijderen, enz.) duurt. Deze tijd omvat de verwerking van de bewerking door de Bus Service service naast de latentie van de aanvraag en het antwoord. Om het aantal bewerkingen per keer te verhogen, moeten tegelijkertijd bewerkingen uitvoeren. Zie [Aanbevolen procedures voor prestaties verbeteringen met behulp van Service Bus Brokered Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx)voor meer informatie.

### <a name="solution"></a>Oplossing

Zie de [Klasse QueueClient (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) voor informatie over het gebruik van de aanbevolen methode voor asynchrone.

Zie het ontwerppatroon [Primer voor asynchrone berichtafhandeling](https://msdn.microsoft.com/library/dn589781.aspx)ter verbetering van de prestaties van de Azure messaging-infrastructuur.

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Houd rekening met partitionering Service Bus wachtrijen en onderwerpen

### <a name="id"></a>ID

AP2004

### <a name="description"></a>Beschrijving

Partitie Service Bus wachtrijen en onderwerpen voor betere prestaties bij messaging Service Bus.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Partitionering Service Bus wachtrijen en onderwerpen verhoogt de prestaties doorvoer en service beschikbaarheid omdat de algehele doorvoer van een gepartitioneerde wachtrij of een onderwerp niet langer wordt beperkt door de prestaties van een enkel bericht broker of berichtenarchief. Bovendien maakt een tijdelijke storing van een berichtenarchief niet een gepartitioneerde wachtrij of onderwerp niet beschikbaar. Zie [Berichten entiteiten partitioneren](https://msdn.microsoft.com/library/azure/dn520246.aspx)voor meer informatie.

### <a name="solution"></a>Oplossing

Het volgende stukje code laat zien hoe partitioneren messaging entiteiten.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Zie voor meer informatie [gepartitioneerd Bus servicewachtrijen en onderwerpen | Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) en check het monster [Microsoft Azure Service Bus gepartitioneerd wachtrij](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) .

## <a name="do-not-set-sharedaccessstarttime"></a>Stel niet SharedAccessStartTime

### <a name="id"></a>ID

AP3001

### <a name="description"></a>Beschrijving

Vermijd SharedAccessStartTimeset gebruiken om de huidige tijd onmiddellijk starten van het beleid van de toegang tot gedeelde. U hoeft alleen deze eigenschap instellen als u wilt beginnen met het beleid voor gedeelde toegang op een later tijdstip.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Kloksynchronisatie veroorzaakt een lichte tijdsverschil tussen datacenters. Bijvoorbeeld logisch zou je denken als u de begintijd van een beleid van SAS-opslag als de huidige tijd via DateTime.Now of een vergelijkbare methode resulteert in het SAS-beleid worden onmiddellijk van kracht. Echter, de geringe tijd verschillen tussen datacenters kunnen problemen veroorzaken met dit omdat sommige momenten datacenter enigszins hoger zijn dan de begintijd, terwijl anderen op het. Als gevolg hiervan het SAS-beleid kan verlopen snel (of zelfs onmiddellijk) als de levensduur van het beleid te kort is ingesteld.

Zie voor meer informatie over het gebruik van gedeelde toegang handtekening op Azure opslag [Introductie van tabel SAS (gedeelde toegang handtekening), wachtrij SAS en update van Blob SAS - opslag Team Blog voor Microsoft Azure - Site-Home - MSDN-weblogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Oplossing

De instructie die de begintijd van het beleid voor gedeelde toegang te verwijderen. Met het analysehulpprogramma Azure Code biedt een correctie voor dit probleem. Zie voor meer informatie over het beheren van de beveiliging van het ontwerppatroon [Valet Key patroon](https://msdn.microsoft.com/library/dn568102.aspx).

Het volgende codefragment ziet u de code oplossing voor dit probleem.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Gedeelde-beleid verstrijken tijd langer dan vijf minuten moet.

### <a name="id"></a>ID

AP3002

### <a name="description"></a>Beschrijving

Kan er maar liefst vijf minuten verschil in klokken tussen datacenters op verschillende locaties als gevolg van de zogeheten "tijdsverschil." Om te voorkomen dat de SA's ingesteld beleid token verlopen eerder dan gepland, de verlooptijd voor meer dan vijf minuten.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Datacenters op verschillende locaties over de hele wereld worden gesynchroniseerd met een kloksignaal. Omdat het duurt voor de kloksignaal voor een reis naar verschillende locaties, kan er een afwijking in de tijd tussen datacenters op verschillende geografische locaties Hoewel alles zogenaamd wordt gesynchroniseerd. Dit tijdsverschil kan invloed hebben op de toegang tot gedeelde beleid start en de vervaldatum interval. Daarom, om toegang tot gedeelde beleid onmiddellijk van kracht wordt, geeft u de begintijd. Controleer bovendien of dat de verlooptijd is meer dan 5 minuten om te voorkomen dat de vroege time-out.

Zie voor meer informatie over het gebruik van gedeelde toegang handtekening op Azure opslag [Introductie van tabel SAS (gedeelde toegang handtekening), wachtrij SAS en update van Blob SAS - opslag Team Blog voor Microsoft Azure - Site-Home - MSDN-weblogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Oplossing

Zie voor meer informatie over het beheren van de beveiliging van het ontwerppatroon [Valet Key patroon](https://msdn.microsoft.com/library/dn568102.aspx).

Hier volgt een voorbeeld van een begintijd voor toegang tot gedeelde beleid niet op te geven.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Hier volgt een voorbeeld van het opgeven van een begintijd voor toegang tot gedeelde beleid met een duur van beleid vervaldatum meer dan vijf minuten.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Zie voor meer informatie, [maken en gebruiken van een gedeelde Access-handtekening](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>CloudConfigurationManager gebruiken

### <a name="id"></a>ID

AP4000

### <a name="description"></a>Beschrijving

Met behulp van de klasse [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) voor projecten zoals Azure Website en Azure mobiele services won't leiden tot runtime-problemen. Beste is het echter verstandig wolk[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) gebruiken als een eenduidige manier van configuraties voor alle Azure Cloud toepassingen beheren.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

CloudConfigurationManager leest het configuratiebestand voor de omgeving.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Oplossing

Refactoring van de code voor het gebruik van de [Klasse CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Een code oplossing voor dit probleem wordt geleverd door het hulpmiddel Azure Code.

Het volgende codefragment ziet u de code oplossing voor dit probleem. Vervangen

`var settings = ConfigurationManager.AppSettings["mySettings"];`

met

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Hier volgt een voorbeeld van hoe u de configuratie-instelling in een App.config- of Web.config-bestand opslaat. De instellingen toevoegen aan de sectie appSettings van het configuratiebestand. Het volgende is het bestand Web.config voor de in het vorige voorbeeld.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Vermijd het gebruik van hardcoded verbindingsreeksen

### <a name="id"></a>ID

AP4001

### <a name="description"></a>Beschrijving

Als u hardgecodeerde verbindingsreeksen en moet u deze later bijwerken, hebt u wijzigingen aanbrengen in de broncode en compileren van de toepassing. Echter, als u de verbindingsreeks in een configuratiebestand opslaan, kunt u ze later door simpelweg het configuratiebestand wordt bijgewerkt.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Harde codering verbindingsreeksen is een slechte gewoonte, omdat het problemen geeft wanneer verbindingsreeksen moeten snel worden gewijzigd. Ook als het project worden ingecheckt moet in bronbeheer, vastgelegde verbindingsreeksen leiden tot beveiligingsproblemen sinds de tekenreeksen kunnen worden bekeken in de broncode.

### <a name="solution"></a>Oplossing

Verbindingstekenreeksen worden opgeslagen in configuratiebestanden of Azure omgevingen.

- Voor zelfstandige toepassingen app.config te gebruiken voor het opslaan van instellingen voor connection strings.

- Gebruik voor IIS gehost webtoepassingen web.config voor het opslaan van tekenreeksen.

- Gebruik configuration.json voor het opslaan van tekenreeksen voor ASP.NET-toepassingen vNext.

Voor meer informatie over het gebruik van bestanden zoals web.config of app.config configuraties Zie [ASP.NET Web configuratie-instructies](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx). Zie voor informatie over hoe Azure omgeving variabelen werken, [websites Azure: hoe tekenreeksen van toepassingen en verbinding tekenreeksen werken](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Zie [voorkomen dat gevoelige informatie zoals verbindingsreeksen in bestanden die zijn opgeslagen in de bron code repository plaatsen](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)voor meer informatie over het opslaan van de verbindingsreeks in het besturingselement.

## <a name="use-diagnostics-configuration-file"></a>Diagnostische gegevens van het configuratiebestand gebruiken

### <a name="id"></a>ID

AP5000

### <a name="description"></a>Beschrijving

In plaats van diagnostische instellingen configureren in de code, zoals met behulp van de programmering van de API Microsoft.WindowsAzure.Diagnostics, moet u de diagnostische instellingen configureren in het bestand diagnostics.wadcfg. (Of diagnostics.wadcfgx als u Azure SDK 2.5). Op deze manier kunt u diagnostische instellingen zonder de code opnieuw te compileren.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Voordat de Azure SDK 2.5 (die gebruikmaakt van Azure diagnostics 1.3), Azure Diagnostics (af) kan worden geconfigureerd met behulp van verschillende methoden: toe te voegen aan de configuratie van blob in opslag met imperatieve code, declaratieve configuratie of de standaardconfiguratie. Gebruik een XML-configuratiebestand (diagnostics.wadcfg of diagnositcs.wadcfgx SDK 2.5 en hoger) in het toepassingsproject is echter de beste manier om diagnostische gegevens configureren. Bij deze benadering werkt kan het bestand diagnostics.wadcfg volledig definieert de configuratie en worden gewijzigd en opnieuw geïmplementeerd op zal. Het gebruik van het configuratiebestand diagnostics.wadcfg mengen met de programmatische van configuraties instellen met behulp van de klassen [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)of [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)kan leiden tot verwarring. Zie [initialiseren of Azure Diagnostics-configuratie wijzigen](https://msdn.microsoft.com/library/azure/hh411537.aspx) voor meer informatie.

Vanaf af 1.3 (opgenomen in Azure SDK 2.5) is niet langer code gebruiken voor het configureren van diagnostische gegevens. Daardoor kunt u alleen de configuratie bij het toepassen of bijwerken van de uitbreiding van de diagnostische gegevens voorzien.

### <a name="solution"></a>Oplossing

De ontwerper van de configuratie van diagnostische gegevens diagnostische instellingen verplaatsen naar het configuratiebestand diagnostics (diagnositcs.wadcfg of diagnositcs.wadcfgx SDK 2.5 en hoger) gebruiken. Het verdient ook [2.5 van Azure SDK](http://go.microsoft.com/fwlink/?LinkId=513188) te installeren en de meest recente diagnostische gegevens te gebruiken.

1. Kies Eigenschappen in het snelmenu voor de rol die u wilt configureren en klik op het tabblad configuratie.

1. Zorg dat het selectievakje **Diagnostics inschakelen** is geselecteerd in de sectie **Diagnostische gegevens** .

1. Klik op de knop **configureren** .

  ![Toegang tot de optie diagnose inschakelen](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  Zie [Diagnostische gegevens voor Azure Cloud Services en virtuele Machines configureren](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) voor meer informatie.


## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Voorkomen dat objecten als statische DbContext declareren

### <a name="id"></a>ID

AP6000

### <a name="description"></a>Beschrijving

Om geheugen te besparen, te voorkomen dat objecten als statische DBContext declareren.

Neem deel uw ideeën en feedback op de [feedback van Azure Code analyse](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

DBContext-objecten bevatten van elke oproep van de queryresultaten. Statische DBContext objecten worden niet verwijderd totdat het toepassingsdomein verwijderd wordt. Een statisch object in de DBContext kan daarom grote hoeveelheden geheugen verbruiken.

### <a name="solution"></a>Oplossing

DBContext declareren als een lokale variabele of een van de niet-statisch exemplaarveld en deze gebruiken voor een taak laten na gebruik worden verwijderd.

In het volgende voorbeeld MVC controller klasse ziet u het gebruik van het DBContext-object.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over optimzing en het oplossen van problemen apps in Azure, Zie [problemen met een web app in Azure App-Service met behulp van Visual Studio](./app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
