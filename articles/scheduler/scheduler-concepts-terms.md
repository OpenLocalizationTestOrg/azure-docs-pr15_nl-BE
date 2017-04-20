<properties
 pageTitle="Taakplanner-begrippen, termen en entiteiten | Microsoft Azure"
 description="Azure Scheduler concepten, termen en entiteitenhiërarchie, inclusief taken en collecties van de taak.  Ziet u een uitgebreid voorbeeld van een geplande taak."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>Taakplanner-begrippen, terminologie + entiteitenhiërarchie

## <a name="scheduler-entity-hierarchy"></a>Scheduler entiteitenhiërarchie

De volgende tabel worden de belangrijkste bronnen blootgesteld of die worden gebruikt door de Taakplanner-API:

|Resource | Beschrijving |
|---|---|
|**Project collectie**|Een job-collectie bevat een groep taken en onderhoudt instellingen, quota en throttles die worden gedeeld door de taken in de collectie. De collectie van een taak wordt gemaakt door de eigenaar van een abonnement en groepen taken samen op basis van de grenzen of de toepassing. Het is beperkt tot één gebied. U kunt ook de naleving van de quota voor het beperken van het gebruik van alle taken in die collectie. De quota zijn MaxJobs en MaxRecurrence.|
|**Taak**|Een taak definieert een enkele terugkerende actie met eenvoudige of complexe strategieën voor de uitvoering. Acties kunnen HTTP-, opslag-wachtrij, service bus wachtrij of bus onderwerp aanvragen bevatten.|
|**Overzicht van functies**|Geschiedenis van een vertegenwoordigt details voor de uitvoering van een taak. Het bevat succes versus storing als antwoord gegevens.|

## <a name="scheduler-entity-management"></a>Scheduler-beheer van entiteit

Op een hoog niveau de Taakplanner en de API voor het servicebeheer de volgende bewerkingen uit op de resources worden blootgesteld:

|Mogelijkheden|Beschrijving en URI-adres|
|---|---|
|**Taakbeheer-collectie**|GET, PUT, en ondersteuning voor het maken en wijzigen van taak collecties en de daarin opgenomen taken verwijderen. Een collectie van de taak is een container voor taken en toewijzingen naar quota en gedeelde instellingen. Voorbeelden van quota's die later worden beschreven zijn maximum aantal taken en kleinste interval van het terugkeerpatroon. <p>PLAATSEN en verwijderen:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>Toevoegen:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p>
|**Taakbeheer**|GET, PUT, boeken, PATCH en ondersteuning voor het maken en wijzigen van taken verwijderd. Alle taken moeten behoren tot een job-collectie die al bestaat, zodat er geen impliciete maken. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p>|
|**Taakbeheer-overzicht**|ONDERSTEUNING voor 60 dagen van de geschiedenis kan worden uitgevoerd, zoals de verstreken tijd van taak en taak uitvoeren van resultaten ophalen. Query string parameter wordt ondersteuning toegevoegd voor filteren op basis van de toestand en status. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p>|

## <a name="job-types"></a>Taaktypen

Er zijn meerdere soorten taken: http-taken (met inbegrip van HTTPS-taken die SSL ondersteunen), opslag van wachtrijtaken service bus wachtrijtaken en service bus onderwerp taken. HTTP-taken zijn ideaal als u een eindpunt van een bestaande werklast of service. U kunt opslag wachtrijtaken om berichten naar wachtrijen, opslag, zodat deze taken zijn ideaal voor de werklast die wachtrijen opslag gebruiken. Service bus taken zijn ook ideaal voor werklast die service bus wachtrijen onderwerpen en.

## <a name="the-job-entity-in-detail"></a>De entiteit 'taak' in detail

Een geplande taak heeft op een basisniveau, de verschillende onderdelen:

- De actie uit te voeren wanneer de timer van de taak wordt gestart  

- (Optioneel) De tijd voor het uitvoeren van de taak  

- (Optioneel) Wanneer en hoe vaak de taak herhalen  

- (Optioneel) Een actie die moet worden gestart als de primaire actie mislukt  

Een geplande taak bevat ook intern systeem geleverde gegevens zoals de volgende geplande uitvoeringstijd.

De volgende code geeft een uitgebreid voorbeeld van een geplande taak. In de volgende secties vindt u meer informatie.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Zoals in het voorbeeld hierboven met geplande taak, heeft een definitie van de verschillende onderdelen:

- Begintijd ('starttijd')  

- Actie (""), waarin de actie bij fout ("errorAction")

- Herhaling ("herhaling")  

- Toestand ("state")  

- Status ("status")  

- Opnieuw beleid ("retryPolicy")  

We bekijken deze in detail:

## <a name="starttime"></a>Starttijd

De 'starttijd' is de begintijd en de beller een tijdzone opgeeft op de kabel in de [indeling ISO 8601](http://en.wikipedia.org/wiki/ISO_8601)staat.

## <a name="action-and-erroraction"></a>actie- en errorAction

De "actie" is de actie die wordt aangeroepen voor elk exemplaar en een beschrijving van een soort service aanroepen. De actie is wat zal worden uitgevoerd volgens de opgegeven planning. Planner ondersteunt HTTP, storage queue service bus onderwerp en service bus wachtrij acties.

De actie in het bovenstaande voorbeeld is een HTTP-actie. Hieronder volgt een voorbeeld van een wachtrij opslag actie:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Hieronder volgt een voorbeeld van een service bus onderwerp actie.

  "actie": {"type": "serviceBusTopic", "serviceBusTopicMessage": {"topicPath": "t1",  
      'namespace': 'mySBNamespace', "transportType": "netMessaging", / / netMessaging of AMQP 'verificatie': {"sasKeyName": "QPolicy", "type": "sharedAccessKey"}, "bericht": "Een bericht", "brokeredMessageProperties": {}, "customMessageProperties": {"appname": "FromScheduler"}},}

Hieronder volgt een voorbeeld van een actie service bus wachtrij:


  "actie": {"serviceBusQueueMessage": {"wachtrijnaam": "q1",  
      'namespace': 'mySBNamespace', "transportType": "netMessaging", / / netMessaging of AMQP 'verificatie': {  
        "sasKeyName": "QPolicy", "type": "sharedAccessKey"}, "bericht": "Een bericht"  
      "brokeredMessageProperties": {}, "customMessageProperties": {"appname": "FromScheduler"}}, "type": "serviceBusQueue"}

De "errorAction" is de foutafhandeling, wordt de actie die wordt gestart wanneer de primaire actie mislukt. Deze variabele kunt u een eindpunt foutafhandeling belt of stuurt u een bericht voor de gebruiker. Dit kan worden gebruikt voor het bereiken van een secundaire eindpunt in het geval de primaire is niet beschikbaar (bijvoorbeeld in het geval van een ramp op de site van het endpoint) of kan worden gebruikt voor het melden van een eindpunt voor foutafhandeling. Net als de primaire actie is de actie bij fout eenvoudige of samengestelde logica op basis van andere acties. Raadpleeg voor meer informatie het maken van een token SAS [maken](https://msdn.microsoft.com/library/azure/jj721951.aspx)en gebruiken van een gedeelde Access-handtekening.

## <a name="recurrence"></a>terugkeerpatroon

Herhaling heeft verschillende onderdelen:

- Frequentie: Een van de minuut, uur, dag, week, maand, jaar  

- -Interval: Interval met de opgegeven frequentie voor het terugkeerpatroon  

- Voorgeschreven schema: Geef de minuten, uren, weekdagen, maanden en monthdays van het terugkeerpatroon  

- Aantal: Het aantal exemplaren  

- Eindtijd: geen taken worden niet uitgevoerd na de opgegeven eindtijd  

Als het een terugkerende object dat is opgegeven in de definitie van JSON heeft terugkerend een taak. Als u zowel het aantal als eindtijd opgeeft, wordt de voltooiing regel die zich het eerst gehonoreerd.

## <a name="state"></a>staat

De status van de taak is een van de vier waarden: ingeschakeld, uitgeschakeld, voltooid of mislukt. U kunt plaatsen of PATCH taken dat ze bijgewerkt naar de status ingeschakeld of uitgeschakeld. Als een taak is voltooid of mislukt, is dat een definitieve status die niet kan worden bijgewerkt (als de taak nog steeds kan worden verwijderd). Een voorbeeld van de eigenschap state is als volgt:


        "state": "disabled", // enabled, disabled, completed, or faulted
Voltooide en mislukte taken worden na 60 dagen verwijderd.

## <a name="status"></a>status

Eenmaal een job Scheduler is gestart, wordt informatie over de huidige status van de taak als resultaat gegeven. Dit object kan niet worden ingesteld door de gebruiker, door het systeem ingesteld. Het is echter opgenomen in het taakobject (in plaats van een afzonderlijke resource gekoppelde) zodat u kunt een eenvoudig de status van een taak opvragen.

Taakstatus omvat de tijd van de vorige uitvoering (indien aanwezig), de tijd van de volgende geplande uitvoering (voor taken in uitvoering) en het aantal van de uitvoering van de taak.

## <a name="retrypolicy"></a>retryPolicy

Als een geplande taak mislukt, is het mogelijk om op te geven van een beleid opnieuw om te bepalen of en hoe de actie opnieuw is uitgevoerd. Dit wordt bepaald door het **retryType** -object, dat is ingesteld op **geen als er geen beleid opnieuw, zoals hierboven** . Geef **vaste** als er een beleid voor het opnieuw proberen.

Een beleid voor het opnieuw instellen, twee extra instellingen kunnen worden opgegeven: een interval voor opnieuw proberen (**retryInterval**) en het aantal pogingen (**retryCount**).

Het interval voor nieuwe pogingen, opgegeven met het object **retryInterval** is het interval tussen nieuwe pogingen. De standaardwaarde is 30 seconden, de minimumwaarde is configureerbaar is 15 seconden en de maximumwaarde is 18 maanden. Taken in Project gratis collecties hebben een configureerbare minimumwaarde van 1 uur.  Dit is gedefinieerd in de indeling ISO 8601. Ook is de waarde van het aantal nieuwe pogingen opgegeven met het object **retryCount** . het is het aantal keren dat een nieuwe poging wordt gedaan. De standaardwaarde is 4 en de maximale waarde is 20\. zowel **retryInterval** en **retryCount** zijn optioneel. Ze krijgen hun standaard waarden als **retryType** is ingesteld op **vaste** en niet expliciet zijn opgegeven.

## <a name="see-also"></a>Zie ook

 [Wat is Taakplanner?](scheduler-intro.md)

 [Aan de slag met de planner in Azure portal](scheduler-get-started-portal.md)

 [Plannen en facturering in Azure planner](scheduler-plans-billing.md)

 [Het bouwen van complexe schema's en geavanceerde herhaling met Azure Scheduler](scheduler-advanced-complexity.md)

 [Azure Scheduler REST API: naslag](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlets verwijzing](scheduler-powershell-reference.md)

 [Azure Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Azure Scheduler limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Uitgaande verificatie van Azure Scheduler](scheduler-outbound-authentication.md)
