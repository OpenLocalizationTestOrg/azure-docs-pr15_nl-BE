<properties 
    pageTitle="App Service API app triggers | Microsoft Azure" 
    description="Triggers implementeren in een App API in Azure App-Service" 
    services="logic-apps" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="rachelap"/>

# <a name="azure-app-service-api-app-triggers"></a>Azure App Service API app triggers

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van API apps 2014-12-01-voorbeeld.


## <a name="overview"></a>Overzicht

In dit artikel wordt uitgelegd hoe API app triggers implementeren en verbruiken ze een app logica.

Alle van de codefragmenten in dit onderwerp worden gekopieerd van de [FileWatcher API App codevoorbeeld](http://go.microsoft.com/fwlink/?LinkId=534802). 

Overigens moet je de volgende nuget downloaden voor de code in dit artikel bouwen en uitvoeren: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Wat zijn API app triggers?

Het is een gangbare scenario voor een API-app naar een gebeurtenis zodat clients van de API-app de juiste actie in reactie op de gebeurtenis ondernemen kunnen wordt gestart. De REST API gebaseerd mechanisme dat dit scenario ondersteunt wordt een trigger API app genoemd. 

Bijvoorbeeld, stel uw clientcode gebruikmaakt van de [Connector API Twitter app](../app-service-logic/app-service-logic-connector-twitter.md) en uw code nodig zijn voor een actie op basis van nieuwe tweets die bepaalde woorden bevatten. U kunt in dit geval een poll of push-signaal instellen om deze behoefte te vergemakkelijken.

## <a name="poll-trigger-versus-push-trigger"></a>Poll trigger versus push-signaal

Op dit moment worden twee soorten triggers ondersteund:

- Poll trigger - Client controleert de app API voor melding van een gebeurtenis die is gestart. 
- Push-signaal - Client wordt door de API-app gewaarschuwd wanneer een gebeurtenis wordt gestart 

### <a name="poll-trigger"></a>Poll-trigger

Een peiling trigger wordt geïmplementeerd als een gewone REST API en verwacht dat de clients (zoals een app logica) poll om kennisgeving. Terwijl de client staat handhaven kan, is de trigger poll zelf stateless. 

De volgende informatie met betrekking tot de aanvraag en het antwoord pakketten laten sommige belangrijke aspecten van het contract poll trigger:

- Aanvraag
    - HTTP-methode: ophalen
    - Parameters
        - triggerState - dit is een optionele parameter kan clients op te geven dat hun toestand zodat de trigger poll correct of kennisgeving bepalen kunt of niet retourneren op basis van de status.
        - API-specifieke parameters
- Reactie
    - Status code **200** - geldig is en er is een melding van de trigger. De inhoud van de melding is het hoofdgedeelte van de response. Een koptekst 'Opnieuw proberen na' in het antwoord geeft aan dat gegevens aanvullende kennisgeving moet worden opgehaald met een aanroep van de volgende aanvraag.
    - Status code **202** - aanvraag geldig is, maar er is geen nieuwe kennisgeving van de trigger.
    - Status code **4xx** - aanvraag is niet geldig. De client moet de aanvraag niet opnieuw.
    - Status code **5xx** - verzoek heeft geleid tot een interne serverfout en/of een tijdelijk probleem. De client moet de aanvraag opnieuw.

Het volgende stukje code is een voorbeeld van het implementeren van een trigger poll.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

Deze trigger poll testen, volg deze stappen:

1. Implementatie van de API-App een **publiek anonieme**verificatie is ingesteld.
2. Bel de bewerking **aanraken** om een bestand raakt. De volgende afbeelding toont een voorbeeld van een aanvraag via de Postman.
   ![Oproep Touch bewerking via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. De trigger poll aanroepen met de parameter **triggerState** is ingesteld op een tijdstempel dan stap #2. De volgende afbeelding toont een voorbeeld van de aanvraag via Postman.
   ![Oproep Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Push-signaal

Een push-signaal is geïmplementeerd als een gewone REST API die meldingen aan clients die zich hebben geregistreerd duwt op de hoogte gesteld wanneer er specifieke gebeurtenissen gestart.

De volgende informatie met betrekking tot de aanvraag en het antwoord pakketten sommige belangrijke aspecten van het contract push trigger illustreren.

- Aanvraag
    - HTTP-methode: plaatsen
    - Parameters
        - triggerId: vereist - ondoorzichtige tekenreeks (zoals een GUID) met de registratie van een push-signaal.
        - callbackUrl: vereist - URL van de callback moet worden aangeroepen wanneer de gebeurtenis wordt gestart. De aanroep is een eenvoudige HTTP POST-verzoek.
        - API-specifieke parameters
- Reactie
    - Status code **200** - aanvraag voor een succesvolle client registreren.
    - Status code **4xx** - aanvraag is niet geldig. De client moet de aanvraag niet opnieuw.
    - Status code **5xx** - verzoek heeft geleid tot een interne serverfout en/of een tijdelijk probleem. De client moet de aanvraag opnieuw.
- Terugbellen
    - HTTP-methode: POST
    - Instantie verzoeken: berichtinhoud.

Het volgende stukje code is een voorbeeld van het implementeren van een push-signaal:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

Deze trigger poll testen, volg deze stappen:

1. Implementatie van de API-App een **publiek anonieme**verificatie is ingesteld.
2. Ga naar [http://requestb.in/](http://requestb.in/) voor het maken van een RequestBin die als uw URL terugbellen fungeren zal.
3. Bel het push-signaal met een GUID als **triggerId** en de URL van de RequestBin als **callbackUrl**.
   ![Oproep Push-signaal via de Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Bel de bewerking **aanraken** om een bestand raakt. De volgende afbeelding toont een voorbeeld van een aanvraag via de Postman.
   ![Oproep Touch bewerking via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Controleer de RequestBin om te bevestigen dat de retouraanroep push trigger wordt aangeroepen met de eigenschap uitvoer.
   ![Oproep Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Beschrijven van triggers in API, definitie

Na uitvoering van de triggers en implementeren van uw app API naar Azure, Ga naar de **Definitie van API** -blade in Azure preview portal en ziet u triggers worden automatisch herkend in de gebruikersinterface wordt aangedreven door de Swagger 2.0 API-definitie van de API-app.

![API definitie Blade](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Als u op de knop **Downloaden Swagger** en de JSON-bestand opent, ziet u de resultaten van de volgende strekking:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

De extensie eigenschap **x-ms-schedular-trigger** is hoe triggers worden beschreven in de definitie van de API en wordt automatisch door de gateway API app toegevoegd wanneer u om de definitie van de API via de gateway als de aanvraag aan een van de volgende criteria. (U kunt ook deze eigenschap handmatig toevoegen.)

- Poll-trigger
    - Als de HTTP-methode **GET**.
    - Als de eigenschap **bewerkingsnummer** de tekenreeks **trigger bevat**.
    - Als de eigenschap **parameters** een parameter met een eigenschap **name** is ingesteld op **triggerState bevat**.
- Push-signaal
    - Als de HTTP-methode **geplaatst is**.
    - Als de eigenschap **bewerkingsnummer** de tekenreeks **trigger bevat**.
    - Als de eigenschap **parameters** een parameter met een eigenschap **name** is ingesteld op **triggerId bevat**.

## <a name="use-api-app-triggers-in-logic-apps"></a>API app triggers gebruiken in Logic apps

### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Weergeven en configureren van API app triggers in de ontwerpfunctie voor logica apps

Als u een app logica in dezelfde bronnengroep als de API app maakt, zal u worden toe te voegen aan het canvas designer gewoon door erop te klikken. De volgende afbeeldingen illustreren dit:

![Triggers in logica App Designer](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Poll-Trigger in logica App Designer configureren](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Push-signaal configureren in logica App Designer](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>API app triggers voor logica apps optimaliseren

Nadat u triggers aan een API-app toevoegen, zijn er een paar dingen die u doen kunt om de ervaring te verbeteren bij gebruik van de API-app in een app logica.

De parameter **triggerState** voor triggers poll moet bijvoorbeeld worden ingesteld op de volgende expressie in de logica app. Deze expressie moet de laatste aanroep van de trigger van de logica app evalueren en die waarde als resultaat.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Opmerking: Voor een uitleg van de functies die in de voorgaande expressie worden gebruikt, Raadpleeg de documentatie van [Logica App Workflow Definition Language](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Logica app-gebruikers moeten de expressie hierboven voor de parameter **triggerState** tijdens het gebruik van de trigger. Het is mogelijk om deze waarde vooraf ingesteld door de ontwerper van de logica app via de extensie eigenschap **x ms-scheduler aanbeveling**.  De **x-ms -** extensie zichtbaarheidseigenschap kan worden ingesteld op een waarde van *interne* zodat de parameter zelf niet in de ontwerpfunctie weergegeven wordt.  In het volgende fragment wordt geïllustreerd dat.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

Voor push-signalen, de **triggerId** -parameter moet een unieke identificatie de app logica. Aanbevolen best practice is deze eigenschap ingesteld op de naam van de werkstroom met de volgende expressie:

    @workflow().name

Met de eigenschappen van de extensie **x ms-scheduler aanbeveling** en **x-ms-zichtbaarheid** in de API afgebakend, kunt de API-app overbrengen naar de ontwerpfunctie logica app deze expressie voor de gebruiker automatisch ingesteld.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Extensie-eigenschappen toevoegen in API-definition

Extra metagegevens informatie - zoals de extensie eigenschappen **x, ms, scheduler, aanbeveling** en de **zichtbaarheid van x ms** - kan worden toegevoegd in de API-definition op twee manieren: statische of dynamische.

Voor statische metagegevens, kunt u rechtstreeks bewerken van het bestand */metadata/apiDefinition.swagger.json* in uw project en de eigenschappen handmatig toevoegen.

Voor de API apps met metagegevens voor dynamische, kunt u het bestand SwaggerConfig.cs om toe te voegen, een bewerking met het filter dat u deze extensies kunt toevoegen.

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Hier volgt een voorbeeld van hoe deze klasse ter vergemakkelijking van de metagegevens voor dynamische scenario kan worden geïmplementeerd.

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 
