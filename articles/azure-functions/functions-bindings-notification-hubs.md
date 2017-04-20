<properties
    pageTitle="Azure functies melding Hub binding | Microsoft Azure"
    description="Begrijpen hoe Azure melding Hub binding in Azure functies."
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
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Azure functies melding Hub uitvoer binding

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe te configureren en code Azure melding Hub bindingen in Azure functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Uw functies kunnen verzenden met behulp van een geconfigureerde Azure melding Hub met een paar regels code push-meldingen. De kennisgeving Azure Hub moet echter voor de Platform meldingen Services (PNS) u wilt gebruiken geconfigureerd. Zie [aan de slag met melding Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) en klikt u op het doelplatform client boven voor meer informatie over het configureren van een kennisgeving Azure Hub en ontwikkelen van een clienttoepassingen registreren om meldingen te ontvangen.

De berichten die u verzendt zijn eigen meldingen of Sjabloonmeldingen. Native meldingen zoals deze is geconfigureerd een specifieke mededeling platform als doel de `platform` eigenschap van de binding van de uitvoer. Een melding van de sjabloon kan worden gebruikt voor meerdere platforms.   

## <a name="notification-hub-output-binding-properties"></a>Melding hub uitvoer bindende eigenschappen

Het bestand function.json bevat de volgende eigenschappen:

- `name`: De variabele naam gebruikt in de functiecode voor de melding van de hub.
- `type`: moet zijn ingesteld op *'notificationHub'*.
- `tagExpression`: Tag expressies kunnen u opgeven dat berichten worden bezorgd bij een aantal apparaten die zich hebben geregistreerd voor het ontvangen van meldingen die voldoen aan de labelexpressie.  Zie [Routering en tag expressies](../notification-hubs/notification-hubs-tags-segment-push-message.md)voor meer informatie.
- `hubName`: Naam van de resource melding hub in Azure portal.
- `connection`: Deze verbindingsreeks moet een **Toepassingsinstelling** verbindingsreeks ingesteld op de waarde *DefaultFullSharedAccessSignature* voor de hub van uw melding.
- `direction`: moet zijn ingesteld op *"out"*. 
- `platform`: De platform-eigenschap geeft het platform kennisgeving de doelen van uw melding. Moet een van de volgende waarden:
    - `template`: Dit is het standaard-platform als de eigenschap platform wordt weggelaten uit de uitvoer-binding. Sjabloonmeldingen kunnen worden gebruikt voor elk platform dat is geconfigureerd op de Hub Azure melding. Zie voor meer informatie over het gebruik van sjablonen in het algemeen voor het verzenden van meldingen van cross-platform met een Hub Azure melding [sjablonen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
    - `apns`: Apple Push Notification Service. Voor meer informatie over het configureren van de kennisgeving hub voor APNS en de ontvangst van de kennisgeving in een clienttoepassing Zie [push-meldingen verzenden naar iOS met Azure melding Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
    - `adm`: [Amazon apparaat Messaging](https://developer.amazon.com/device-messaging). Zie voor meer informatie over het configureren van de kennisgeving hub voor ADM en ontvangst van de kennisgeving in een Kindle app [Aan de slag met melding Hubs voor Kindle apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, die de nieuwe versie van GCM, wordt ook ondersteund. Voor meer informatie over het configureren van de hub melding GCM/FCM en de ontvangst van de kennisgeving in een app Android client Zie [push-meldingen verzenden naar Android met Azure melding Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
    - `wns`: [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) gericht op Windows-platforms. Windows Phone 8.1 en hoger wordt ook ondersteund door WNS. Voor meer informatie over het configureren van de kennisgeving hub voor WNS en de ontvangst van de kennisgeving in een app Universal Windows Platform (UWP) Zie [aan de slag met melding Hubs voor Windows universele Platform Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Dit platform ondersteunt Windows Phone 8 en eerdere Windows Phone-platforms. Voor meer informatie over het configureren van de kennisgeving hub voor MPNS en de ontvangst van de kennisgeving in een Windows Phone-app, Zie [push-meldingen verzenden met Azure melding Hubs op Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)
 
Voorbeeld function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>Instellingen voor berichtgeving hub connection string

Een melding hub uitvoer binding gebruikt, moet u de verbindingsreeks voor de hub. Dit kan worden gedaan op het tabblad *integratie* door uw melding hub of een nieuwe maken. 

U kunt ook handmatig een verbindingsreeks voor een bestaande hub toevoegen door een verbindingsreeks voor de *DefaultFullSharedAccessSignature* toe te voegen aan uw hub-melding. Deze verbindingsreeks kunt u de functie-machtiging voor het verzenden van meldingen. De tekenreekswaarde *DefaultFullSharedAccessSignature* verbinding toegankelijk is via de knop **sleutels** in de belangrijkste blade van uw melding hub resource in Azure portal. Een verbindingstekenreeks voor de hub handmatig toevoegen, gebruik de volgende stappen uit: 

1. Klik op de **functie app** blade van de Azure portal, **functie App instellingen > Ga naar de instellingen App**.

2. Klik op **Instellingen voor toepassing**in de blade **Instellingen** .

3. Ga naar de sectie **verbindingsreeksen** en een benoemd item voor *DefaultFullSharedAccessSignature* -waarde voor de hub van uw melding toevoegen. Het type wijzigen in **aangepast**.
4. Verwijzen naar de naam van de verbinding string in de uitvoer-bindingen. Vergelijkbaar met **MyHubConnectionString** die in het bovenstaande voorbeeld wordt gebruikt.

## <a name="native-notification-examples"></a>Voorbeelden van de oorspronkelijke kennisgeving

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>Native meldingen met C# APNS triggers de wachtrij

In dit voorbeeld ziet hoe u met gegevenstypen die zijn gedefinieerd in de [Bibliotheek van Microsoft Azure melding Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) een native APNS bericht verzenden. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>Native meldingen met C# GCM triggers de wachtrij

In dit voorbeeld ziet hoe u met gegevenstypen die zijn gedefinieerd in de [Bibliotheek van Microsoft Azure melding Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) een native GCM-bericht verzenden. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>Native meldingen met C# WNS triggers de wachtrij

In dit voorbeeld ziet u hoe gegevenstypen die zijn gedefinieerd in de [Bibliotheek van Microsoft Azure melding Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) met een eigen WNS toast melding verzenden. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>Voorbeelden van sjabloon melding

#### <a name="template-example-for-nodejs-timer-triggers"></a>Voorbeeld van de sjabloon voor Node.js timer triggers 

In het volgende voorbeeld stuurt een melding voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) met `location` en `message`.

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>Voorbeeld van de sjabloon voor F # timer triggers

In het volgende voorbeeld stuurt een melding voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) met `location` en `message`.

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>Voorbeeld van de sjabloon met behulp van een uitvoerparameter 

In het volgende voorbeeld stuurt een melding voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) met een `message` tijdelijke plaatsaanduiding in de sjabloon.

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>Voorbeeld van een sjabloon met asynchrone functie

Als u asynchrone code zijn genoemde parameters niet toegestaan. In dit geval gebruikt u `IAsyncCollector` om terug te keren uw sjabloon melding. De volgende code is een asynchrone voorbeeld van de bovenstaande code. 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>Voorbeeld van de sjabloon met behulp van JSON 

In het volgende voorbeeld stuurt een melding voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) met een `message` tijdelijke plaatsaanduiding in de sjabloon met behulp van een geldige JSON-tekenreeks.

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>Voorbeeld van de sjabloon met Hubs kennisgeving mediawisselaar, typen

In dit voorbeeld ziet hoe u met gegevenstypen die zijn gedefinieerd in de [Bibliotheek van Microsoft Azure melding Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
