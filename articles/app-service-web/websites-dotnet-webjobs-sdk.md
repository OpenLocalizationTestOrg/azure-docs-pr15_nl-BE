<properties 
    pageTitle="Wat is de WebJobs Azure SDK" 
    description="Een inleiding tot de Azure WebJobs SDK. Legt uit wat de SDK is en typische scenario's is handig voor en voorbeelden van code." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="what-is-the-azure-webjobs-sdk"></a>Wat is de WebJobs Azure SDK

## <a id="overview"></a>Overzicht

In dit artikel wordt uitgelegd wat de WebJobs SDK is, bekijkt enkele algemene scenario's is handig voor en geeft een overzicht van hoe u deze in uw code gebruikt.

[WebJobs](websites-webjobs-resources.md) is een functie van Azure App-Service kunt u een programma of script uitvoeren in de context van een web app, API app of mobiele app. Het doel van de [WebJobs SDK](websites-webjobs-resources.md) is voor het vereenvoudigen van de code die u voor veelvoorkomende taken schrijft die een WebJob kunt uitvoeren, zoals beeldverwerking, verwerking van de wachtrij, RSS aggregatie, onderhoud van bestanden en e-mails verzenden. De WebJobs SDK bevat ingebouwde voorzieningen voor het werken met Azure opslag en Service Bus voor het plannen van taken en het afhandelen van fouten en voor veel andere veelvoorkomende scenario's. Bovendien ontworpen om te worden verlengd. De die wordt [WebJobs SDK open source is](https://github.com/Azure/azure-webjobs-sdk/), en er is een [open source opslagplaats voor uitbreidingen](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

De WebJobs SDK bevat de volgende onderdelen:

* **NuGet pakketten**. NuGet-pakketten die u aan een project van Visual Studio consoletoepassing toevoegt bieden een kader dat in uw code worden gebruikt door het versieren van uw methoden met de kenmerken van de WebJobs SDK.
  
* **Dashboard**. Onderdeel van de WebJobs SDK is opgenomen in Azure App Service en biedt uitgebreide controle en diagnose voor programma's die gebruikmaken van de pakketten NuGet. U hebt geen code schrijven voor het gebruik van deze voorzieningen voor bewaking en diagnose.

## <a id="scenarios"></a>Scenario 's

Hier volgen enkele typische scenario's die u gemakkelijk met de SDK Azure WebJobs verwerken kunt:

* Afbeelding verwerking of andere CPU-intensieve werk. Een veelvoorkomende functie van websites is de mogelijkheid voor het uploaden van afbeeldingen of video's. Vaak wilt u de inhoud bewerken nadat deze geüpload, maar u niet wilt dat de gebruiker wacht terwijl u die maken.

* Verwerking van de wachtrij. Een veelgebruikte methode voor een web-frontend voor communicatie met een back-end-service is het gebruik van wachtrijen. Wanneer de website werk moet, duwt het een bericht naar een wachtrij. Een back-end-service haalt de berichten uit de wachtrij en het werk. U kunt wachtrijen voor beeldverwerking: bijvoorbeeld nadat de gebruiker een aantal bestanden uploadt, de namen van de plaatsen in een bericht wachtrij worden opgevangen door de back-end voor verwerking. Of u kunt wachtrijen reactievermogen site verbeteren. Bijvoorbeeld, in plaats van rechtstreeks naar een SQL-database, schrijven naar een wachtrij, vertelt de gebruiker dat u bent klaar, en laat de back-end-ingang hoge latentie relationele database werken. Zie voor een voorbeeld van een wachtrij met het proces om image processing, de [zelfstudie WebJobs SDK aan de slag](websites-dotnet-webjobs-sdk-get-started.md).

* RSS-aggregatie. Als u een site hebt die een lijst van RSS-feeds houdt, kan u ophalen van alle artikelen van de feeds in de achtergrond.

* Onderhoud van bestanden, zoals aggregeren of logboekbestanden worden opgeruimd.  Mogelijk hebt u logboekbestanden worden gemaakt door verschillende sites of voor de afzonderlijke tijdsperioden die u combineren wilt om de taken van de analyse wordt uitgevoerd. Of u kunt een taak uit te voeren voor het opschonen van oude logboekbestanden wekelijks.

* Ingress in Azure tabellen. U kunt bestanden die zijn opgeslagen als BLOB's en wilt parseren ze en slaat de gegevens op in tabellen. De ingress-functie kan schrijven groot aantal rijen (miljoenen in sommige gevallen) en de WebJobs SDK maakt het mogelijk om deze functionaliteit eenvoudig te implementeren. De SDK bevat ook real-time bewaking van de voortgangsindicatoren zoals het aantal rijen in de tabel wordt geschreven.

* Andere langdurige taken die u wilt uitvoeren in een thread op de achtergrond, zoals het [verzenden van e-mails](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 

* Alle taken die u uitvoeren volgens een schema wilt, zoals een back-up bewerking elke nacht.

In veel van deze scenario's kan u wilt schalen van een webtoepassing uit te voeren op meerdere VMs, die in meerdere WebJobs tegelijk. In sommige gevallen kan dit resulteren in dezelfde gegevens meerdere keren verwerkt, maar dit is geen probleem als u de ingebouwde wachtrij, blob en triggers-Service Bus van de WebJobs SDK. De SDK zorgt ervoor dat uw functies wordt verwerkt slechts eenmaal voor elk bericht of blob.

De WebJobs SDK ook gemakkelijk veelvoorkomende scenario's voor foutafhandeling te verwerken. U kunt waarschuwingen instellen om meldingen te verzenden wanneer een functie is mislukt en u time-outs instellen kunt zodat deze functie automatisch wordt geannuleerd als er binnen een bepaalde termijn niet wordt uitgevoerd.

## <a id="code"></a>Codevoorbeelden

De code voor het verwerken van veelvoorkomende taken die met Azure opslag werken is eenvoudig. In de Console van de toepassing `Main` methode u maakt een `JobHost` -object dat de coördinaten van het aanroepen van methoden u schrijven. Het framework SDK WebJobs weet wanneer uw methoden aangeroepen en parameterwaarden wilt gebruiken op basis van de WebJobs SDK kenmerken die u in deze. De SDK bevat *triggers* die aangeven welke voorwaarden tot gevolg hebben dat de functie die wordt aangeroepen en *bindmiddelen* die opgeeft hoe u gegevens naar en van de methodeparameters.

Het kenmerk [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) wordt bijvoorbeeld een functie die wordt aangeroepen wanneer een bericht wordt ontvangen in een wachtrij en als de berichtindeling JSON voor een matrix van bytes of een aangepast type is, het bericht automatisch gedeserialiseerd. Het kenmerk [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) wordt een proces geactiveerd wanneer er een nieuwe blob wordt gemaakt in een opslag Azure-account.

Dit is een eenvoudig programma waarmee een wachtrij worden opgevraagd en maakt een blob voor elke wachtrij bericht ontvangen:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

De `JobHost` -object is een container voor een aantal functies van de achtergrond. De `JobHost` object de functies, horloges voor gebeurtenissen die worden geactiveerd, gecontroleerd en de functies worden uitgevoerd bij de trigger-gebeurtenissen. U belt een `JobHost` methode om aan te geven of u wilt dat het proces van de container worden uitgevoerd op de huidige thread of een thread op de achtergrond. In het voorbeeld wordt de `RunAndBlock` methode, het proces blijft actief op de huidige thread.

Omdat de `ProcessQueueMessage` methode in dit voorbeeld is een `QueueTrigger` attribuut, de trigger voor die functie is de ontvangst van een nieuw bericht in de wachtrij. De `JobHost` object wacht op nieuwe berichten op de opgegeven wachtrij ('webjobsqueue' in dit voorbeeld) en wanneer er een wordt gevonden, roept `ProcessQueueMessage`. 

De `QueueTrigger` bindingen van het kenmerk de `inputText` parameter de waarde van de wachtrij weergegeven. En de `Blob` kenmerk binds een `TextWriter` object naar een blob met de naam 'blobname' in een container met de naam 'containername'.  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

Vervolgens wordt deze parameters de waarde van het bericht wachtrij naar de blob schrijven:

        writer.WriteLine(inputText);

De trigger en binder functies van de SDK WebJobs vereenvoudigen de code die u moet schrijven. De low-levelcode vereist voor het verwerken van wachtrijen, BLOB's of bestanden, of geplande taken starten doet u door het framework SDK WebJobs. Bijvoorbeeld het framework maakt wachtrijen die niet bestaan, wordt de wachtrij, berichten in de wachtrij leest, verwijdert de wachtrij berichten bij de verwerking is voltooid, maakt u blob-containers die niet bestaan nog, schrijft naar BLOB's, enzovoort.

In het volgende voorbeeld ziet u een groot aantal triggers in één WebJob: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, en `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Planning

De `TimerTrigger` kenmerk kunt u de trigger-functies uit te voeren op een schema. U kunt een WebJob plannen als geheel tot en met de afzonderlijke functies van Azure of planning van een WebJob met de WebJobs SDK `TimerTrigger`. Hier volgt een voorbeeld van code.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

Zie voor meer voorbeelden van code, [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) in de opslagplaats azure-webjobs-sdk-extensies op GitHub.com.

## <a name="extensibility"></a>Uitbreidbaarheid

U bent niet beperkt tot de ingebouwde functie--de WebJobs SDK, kunt u aangepaste triggers en bindmiddelen schrijven.  U kunt bijvoorbeeld triggers voor cachegebeurtenissen en de periodieke's schrijven. Een [open source repository](https://github.com/Azure/azure-webjobs-sdk-extensions) bevat een [gedetailleerde gids op WebJobs SDK uitbreidbaarheid](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) en steekproef code waarmee u schrijft uw eigen triggers en bindmiddelen.

## <a id="workerrole"></a>Met behulp van de SDK WebJobs buiten WebJobs

Een programma dat gebruikmaakt van de de WebJobs SDK is een standaard consoletoepassing en overal kunt uitvoeren--dit hoeft te worden uitgevoerd als een WebJob. U kunt het programma lokaal op uw ontwikkelcomputer testen en in productie kunt u deze uitvoeren in een Cloud Service werknemer rol of een Windows-service als u liever een van deze omgevingen. 

Het dashboard is echter alleen beschikbaar als uitbreiding voor een web app van Azure App-Service. Als u buiten een WebJob uitgevoerd en nog steeds gebruik van het Dashboard wilt, kunt u een web app voor het gebruik van dezelfde opslag account die de WebJobs SDK Dashboard verbindingsreeks verwijst naar en van web app WebJobs Dashboard worden gegevens over de uitvoering van een functie van het programma dat wordt uitgevoerd als ergens anders vervolgens weergegeven. U kunt opvragen voor het Dashboard met de URL https://*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions. Zie [een dashboard voor plaatselijke ontwikkeling bij de WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx)voor meer informatie, maar het weblogbericht wordt een oude naam voor de verbinding. 

## <a id="nostorage"></a>Dashboardfuncties

De WebJobs SDK biedt diverse voordelen, zelfs als u geen triggers WebJobs SDK of bindmiddelen:

* U kunt de functies van het Dashboard aanroepen.
* U kunt de functies van het Dashboard opnieuw afspelen.
* U Logboeken kunt weergeven in het Dashboard, gekoppeld aan de specifieke WebJob (toepassingslogboeken, geschreven met behulp van Console.Out, Console.Error, Trace, enz.) of gekoppeld aan de specifieke functie-aanroep die zij ontstaan (Logboeken geschreven met behulp van een `TextWriter` -object dat de SDK wordt doorgegeven als een parameter aan de functie). 

Zie voor meer informatie [hoe u handmatig een functie aanroepen](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) en [het schrijven van logboekbestanden](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Volgende stappen

Zie voor meer informatie over de WebJobs SDK [Azure WebJobs aanbevolen bronnen](http://go.microsoft.com/fwlink/?linkid=390226).

Zie de [Release-opmerkingen](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)voor meer informatie over de meest recente verbeteringen aan de WebJobs SDK.
 
