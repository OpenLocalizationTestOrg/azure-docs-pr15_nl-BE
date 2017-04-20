<properties
    pageTitle="Azure functies gebeurtenis Hub bindingen | Microsoft Azure"
    description="Begrijpen hoe Azure gebeurtenis Hub bindingen in Azure functies."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure functies, functies, verwerking van gebeurtenissen, dynamische compute, zonder server architecture"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure functies gebeurtenis Hub bindingen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe te configureren en code [Azure gebeurtenis Hub](../event-hubs/event-hubs-overview.md) bindingen voor Azure functies. Azure functies ondersteunen de trigger- en bindingen voor Azure gebeurtenis Hubs.

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Als u bekend met Azure gebeurtenis Hubs bent, Zie het [Overzicht Azure gebeurtenis Hub](../event-hubs/event-hubs-overview.md).

## <a name="azure-event-hub-trigger-binding"></a>Azure gebeurtenis Hub binding activeren

Een trigger Azure gebeurtenis Hub kan worden gebruikt om te reageren op een gebeurtenis die wordt verzonden naar een gebeurtenis hub gebeurtenis stroom. U moet leestoegang hebben tot de gebeurtenis hub voor het instellen van een binding trigger.

#### <a name="functionjson-for-event-hub-trigger-binding"></a>Function.JSON voor gebeurtenis Hub binding activeren

Het bestand *function.json* voor een trigger Azure gebeurtenis Hub bevat de volgende eigenschappen:

- `type`: Het moet worden ingesteld op *eventHubTrigger*.
- `name`: De variabele naam gebruikt in de functiecode voor het gebeurtenisbericht hub. 
- `direction`: *In*moet worden ingesteld. 
- `path`: De naam van de gebeurtenis hub.
- `consumerGroup`: Dit is een optionele eigenschap gebruikt voor het instellen van de [groep van de consument](../event-hubs-overview.md#consumer-groups) zich abonneren op gebeurtenissen in de hub. Als dit argument wordt weggelaten, de `$Default` groep consumenten wordt gebruikt. 
- `connection`: De naam van een instelling die app bevat de verbindingsreeks met de gebeurtenis hub bevindt zich in de naamruimte. Kopieer deze verbindingsreeks door te klikken op de knop van de **Verbindingsgegevens** voor de naamruimte, niet de gebeurtenis hub zelf.  Deze verbindingsreeks moet beschikken over ten minste leesmachtigingen voor de trigger wordt geactiveerd.

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Azure gebeurtenis Hub trigger C#-voorbeeld
 
Met het bovenstaande voorbeeld-function.json, wordt de hoofdtekst van het gebeurtenisbericht geregistreerd met behulp van de C# functiecode hieronder:
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Voorbeeld van de trigger F # Azure gebeurtenis Hub

Met het bovenstaande voorbeeld-function.json, wordt de hoofdtekst van het gebeurtenisbericht geregistreerd met behulp van de F # functiecode hieronder:

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Azure gebeurtenis Hub trigger Node.js voorbeeld
 
Met het bovenstaande voorbeeld-function.json, wordt de hoofdtekst van het gebeurtenisbericht geregistreerd met behulp van de functiecode Node.js:
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Azure gebeurtenis Hub uitvoer binding

Een gebeurtenis Azure Hub uitvoer binding gebeurtenissen schrijven naar een gebeurtenis hub gebeurtenis stroom wordt gebruikt. U moet gemachtigd zijn verzenden op een hub gebeurtenis gebeurtenissen naar het schrijven. 

#### <a name="functionjson-for-event-hub-output-binding"></a>Function.JSON voor gebeurtenis Hub uitvoer binding

Het bestand *function.json* voor een gebeurtenis Azure Hub uitvoer binding bevat de volgende eigenschappen:

- `type`: Het moet worden ingesteld op *eventHub*.
- `name`: De variabele naam gebruikt in de functiecode voor het gebeurtenisbericht hub. 
- `path`: De naam van de gebeurtenis hub.
- `connection`: De naam van een instelling die app bevat de verbindingsreeks met de gebeurtenis hub bevindt zich in de naamruimte. Kopieer deze verbindingsreeks door te klikken op de knop van de **Verbindingsgegevens** voor de naamruimte, niet de gebeurtenis hub zelf.  Deze verbindingsreeks hebt machtigingen verzenden het bericht te verzenden naar de stroom van de Hub van de gebeurtenis.
- `direction`: Moet zijn ingesteld op *uit*. 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure gebeurtenis Hub C#-voorbeeldcode voor het binden van de uitvoer
 
De volgende C# voorbeeld van de functiecode toont een gebeurtenis op een gebeurtenis gebeurtenis Hub stroom schrijven. In dit voorbeeld geeft de gebeurtenis Hub binding weergegeven voor de bovenstaande uitvoer wordt toegepast op een C#-timer trigger.  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure gebeurtenis Hub F #-voorbeeldcode voor het binden van de uitvoer

De volgende F # voorbeeld van de functiecode toont een gebeurtenis op een gebeurtenis gebeurtenis Hub stroom schrijven. In dit voorbeeld geeft de gebeurtenis Hub binding weergegeven voor de bovenstaande uitvoer wordt toegepast op een C#-timer trigger.

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Azure gebeurtenis Hub Node.js voorbeeldcode voor het binden van de uitvoer
 
De volgende Node.js functie voorbeeldcode wordt een gebeurtenis op een gebeurtenis gebeurtenis Hub stroom schrijven. In dit voorbeeld geeft de gebeurtenis Hub uitvoer hierboven afgebeelde binden aan een trigger Node.js timer toegepast.  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
