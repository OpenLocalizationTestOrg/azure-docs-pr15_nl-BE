<properties
    pageTitle="Azure functies Twilio binding | Microsoft Azure"
    description="Begrijpen hoe u Twilio bindingen met Azure functies."
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
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Azure functies Twilio uitvoer binding

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe u kunt configureren en gebruiken van Twilio bindingen met Azure functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Azure functies ondersteunt Twilio uitvoer bindingen zodat de functies voor het verzenden van SMS-berichten met een paar regels code en een [Twilio](https://www.twilio.com/) -account. 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>Function.JSON voor Azure melding Hub uitvoer binding

Het bestand function.json bevat de volgende eigenschappen:

- `name`: De variabele naam gebruikt in de functiecode voor de Twilio SMS-bericht.
- `type`: moet zijn ingesteld op *'twilioSms'*.
- `accountSid`: Deze waarde moet worden ingesteld op de naam van de instelling van een App die in het bezit van uw Twilio Account Sid.
- `authToken`: Deze waarde moet worden ingesteld op de naam van de instelling van een App die in het bezit van de Twilio-verificatietoken.
- `to`: Deze waarde is ingesteld op het telefoonnummer dat naar de SMS-tekst wordt verzonden.
- `from`: Deze waarde is ingesteld op het telefoonnummer van de SMS-tekst wordt verzonden.
- `direction`: moet zijn ingesteld op *"out"*.
- `body`: Deze waarde kan worden gebruikt om harde het SMS-bericht als u geen dynamisch instellen in de code voor uw functie. 

 
Voorbeeld function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Voorbeeld C# wachtrij trigger met Twilio binding uitvoer

#### <a name="synchronous"></a>Synchrone

Deze synchrone voorbeeldcode voor een trigger Azure Storage queue gebruikt een uitvoerparameter een tekstbericht verzenden naar een klant die een order heeft geplaatst.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>Asynchrone

Deze asynchrone voorbeeldcode voor een trigger Azure opslag wachtrij stuurt een SMS-bericht naar een klant die een order heeft geplaatst.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Voorbeeld Node.js wachtrij trigger met Twilio binding uitvoer

In dit voorbeeld Node.js verzendt een bericht aan een klant die een order heeft geplaatst.

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
