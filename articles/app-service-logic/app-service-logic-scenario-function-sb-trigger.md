<properties
   pageTitle="Logica app scenario: een trigger Azure functies Service Bus maken | Microsoft Azure"
   description="Azure-functies gebruiken voor een Service Bus trigger maken voor een app logica"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Logica app scenario: een trigger Azure Service Bus met Azure functies maken

Azure-functies kunt u een trigger maken voor een app logica te implementeren voor een langdurige listener of taak. U kunt bijvoorbeeld een functie die een wachtrij luisteren en een app logica als een push-signaal wordt onmiddellijk geactiveerd maken.

## <a name="build-the-logic-app"></a>Bouwen van de logica-app

In dit voorbeeld hebt u een functie met voor elke logica app dat moet worden gestart. Maak eerst een app logica met een HTTP-aanvraag-trigger. De functie roept dat eindpunt wanneer er een message queue is ontvangen.  

1. Maak een nieuwe logica app; Selecteer de trigger **handleiding - wanneer een HTTP-aanvraag wordt ontvangen** .  
   Desgewenst kunt u een schema JSON te gebruiken met het bericht wachtrij met behulp van een hulpprogramma zoals [jsonschema.net](http://jsonschema.net). Het schema in de trigger te plakken. Hierdoor is de ontwerper van het inzicht in de vorm van de gegevens en meer eigenschappen via de werkstroom gemakkelijk stromen.
1. Voeg eventuele extra stappen die u laten uitvoeren wilt na van een bericht wachtrij ontvangst. Bijvoorbeeld: stuur een e-mail via Office 365.  
1. Sla de app logica voor het genereren van de callback-URL voor de trigger op dit app logica. De URL wordt weergegeven op de kaart van de trigger.

![De callback URL wordt weergegeven op de kaart van de trigger][1]

## <a name="build-the-function"></a>De functie maken

Vervolgens moet u een functie maakt die zal fungeren als de trigger en luister naar de wachtrij.

1. Selecteer een **Nieuwe functie**in de [portal Azure functies](https://functions.azure.com/signin)en en selecteer vervolgens de sjabloon **ServiceBusQueueTrigger - C#** .

    ![Azure functies portal][2]

2. Configureer de verbinding met de Service Bus-wachtrij (zal gebruik maken van de SDK Azure Service Bus `OnMessageReceive()` listener).
3. Schrijf een eenvoudige functie om aan te roepen van de logica app eindpunt (eerder gemaakt) met behulp van het bericht wachtrij als een trigger. Hier is een volledig voorbeeld van een functie. In het voorbeeld wordt een `application/json` type inhoud dat wordt weergegeven, maar u kunt dit desgewenst wijzigen.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Toevoegen als u wilt testen, een bericht wachtrij via een hulpprogramma zoals [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer). Zie de logica app geactiveerd onmiddellijk nadat de functie het bericht ontvangt.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG
