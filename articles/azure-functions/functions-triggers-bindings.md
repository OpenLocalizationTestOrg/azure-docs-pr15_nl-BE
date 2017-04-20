<properties
    pageTitle="Azure functies, triggers en bindingen | Microsoft Azure"
    description="Werken met triggers en bindingen in Azure functies."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure functies, functies, verwerking van de gebeurtenis, webhooks, dynamische compute, zonder server-architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure functies triggers en bindingen: referentie voor ontwikkelaars


Dit onderwerp bevat algemene naslag voor triggers en bindingen. Het bevat enkele van de binding van geavanceerde functies en syntaxis wordt ondersteund door alle bindingstypen.  

Als u gedetailleerde informatie over het configureren en codering van een bepaald type trigger of binding zoekt, kunt u Klik op een van de trigger of de bindingen die hieronder worden beschreven in plaats daarvan:

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

Deze artikelen wordt ervan uitgegaan dat u de [Naslaginformatie voor ontwikkelaars van Azure-functies](functions-reference.md)en de [C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md)of [Node.js](functions-reference-node.md) developer reference artikelen hebt gelezen.



## <a name="overview"></a>Overzicht

Triggers zijn gebeurtenis antwoorden gebruikt voor het starten van uw aangepaste code. Deze kunnen u reageren op gebeurtenissen in de Azure platform of op de lokale. Bindings vertegenwoordigen de nodige meta-gegevens waarmee uw code aansluit op de gewenste trigger of bijbehorende input of outputgegevens.

Als u een beter idee van de verschillende bindingen die u met uw app Azure functie integreren kunt, verwijzen naar de volgende tabel.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

Triggers voor een beter begrip en bindingen in het algemeen, Stel dat u sommige code aan een nieuw item in een wachtrij Azure opslag geplaatst proces uitvoert. Azure functies biedt een trigger de wachtrij Azure om dit te ondersteunen. Moet u, de volgende informatie voor het controleren van de wachtrij:
 
- De opslag rekening waarop de wachtrij zich bevindt.
- De naam van de wachtrij.
- Naam voor een variabele die de code gebruikt om te verwijzen naar het nieuwe item dat in de wachtrij is geplaatst.  
 
De trigger voor een wachtrij binding bevat deze informatie voor een Azure-functie. Het bestand *function.json* voor elke functie bevat alle verwante bindingen.  Hier volgt een voorbeeld van de *function.json* een wachtrij met binding activeren. 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

Uw code kan verschillende soorten uitvoer afhankelijk van hoe de nieuwe wachtrij-item wordt verwerkt sturen. U wilt een nieuwe record aan een tabel Azure opslag schrijven.  Om dit te bereiken, kunt u een binding voor uitvoer naar een tabel Azure opslag instellen. Hier volgt een voorbeeld van de *function.json* met een opslag tabel uitvoer binding met de trigger voor een wachtrij kan worden gebruikt. 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


De volgende C#-functie reageert op een nieuw item in de wachtrij wordt verwijderd en een nieuwe gebruiker post in een tabel Azure opslag geschreven.

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

Zie voor meer voorbeelden van code en meer specifieke informatie over Azure opslagtypen die worden ondersteund, [Azure, functies, triggers en bindingen voor opslag van Azure](functions-bindings-storage.md).


Met de meer geavanceerde functies van de binding in Azure portal, klikt u op de optie **Geavanceerde editor** op het tabblad **integratie** van uw functie. De geavanceerde editor kunt u de *function.json* rechtstreeks in de portal te bewerken.

## <a name="dynamic-parameter-binding"></a>Parameter dynamische binding 

In plaats van een statische configuratie-instelling voor de eigenschappen van de uitvoer-binding, kunt u de instellingen voor het dynamisch zijn gekoppeld aan gegevens die deel uitmaakt van de invoer van de trigger-binding. Houd rekening met een scenario waar nieuwe orders worden verwerkt met behulp van een wachtrij Azure opslag. Elk nieuw item in de wachtrij is een JSON-string met ten minste de volgende eigenschappen:

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

U kunt de klant sturen een SMS-bericht met uw Twilio-account als een update voor dat de order is ontvangen.  U kunt de `body` en `to` veld van uw Twilio uitvoer binding dynamisch gekoppeld is aan de `name` en `mobileNumber` die deel uitmaakten van de invoer als volgt.

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
De functiecode heeft nu alleen als volgt de uitvoerparameter initialiseren. Tijdens de uitvoering van wordt de eigenschappen van de uitvoer gebonden aan de gewenste gegevens zijn ingevoerd.

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

Node.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>Willekeurige GUID 's

Azure functies biedt een syntaxis voor het genereren van willekeurige GUID's met uw bindingen. De volgende syntaxis voor de binding schrijft uitvoer naar een nieuwe BLOB met een unieke naam in een container Azure opslag: 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>Een enkele uitvoer retourneren

In gevallen waarbij de functiecode een enkele uitvoer retourneert kunt u een uitvoer-binding met de naam `$return` een meer natuurlijke functiehandtekening in uw code te behouden. Dit kan alleen worden gebruikt met talen die een retourwaarde (C#, Node.js, F #) ondersteunen. De binding is vergelijkbaar met de volgende uitvoer binding van blob die wordt gebruikt met de trigger voor een wachtrij.

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


De volgende C#-code, wordt de uitvoer natuurlijker zonder gebruik van een `out` parameter in de functiehandtekening.


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


Deze dezelfde aanpak wordt aangetoond hieronder met Node.js.

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F #-voorbeeld hieronder.

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

Dit kan ook worden gebruikt met uitvoerparameters meerdere door het aanwijzen van een enkele uitvoer met `$return`.


## <a name="route-support"></a>Ondersteuning voor route

Wanneer u een functie voor een trigger voor HTTP- of WebHook, is de functie standaard adresseerbare met een route van het formulier:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

U kunt deze route met behulp van de optionele `route` eigenschap in de HTTP-trigger de binding ingevoerd. Als u bijvoorbeeld de volgende *function.json* -bestand definieert een `route` eigenschap voor een HTTP-trigger:

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

Met deze configuratie van is de functie gebruikt met de volgende route in plaats van de oorspronkelijke route.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Hierdoor is de functiecode voor de ondersteuning van twee parameters in het adres, `category` en `id`. U kunt een [Web API Route beperking](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) met de parameters. De volgende C# functiecode gebruik maakt van beide parameters.

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

Hier is de functiecode Node.js dezelfde Routeparameters gebruiken.

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

Alle functie-routes worden standaard voorafgegaan door *api*. Ook aanpassen of verwijderen van het voorvoegsel met behulp van de `http.routePrefix` eigenschap in het bestand *host.json* . In het volgende voorbeeld wordt het routeprefix *api* met behulp van een lege tekenreeks voor het voorvoegsel in het bestand *host.json* verwijderd.

    {
      "http": {
        "routePrefix": ""
      }
    }

Voor gedetailleerde informatie over het bijwerken van het bestand *host.json* voor uw functie, Zie, [hoe de functie app update bestanden](functions-reference.md#fileupdate). 

Door deze configuratie toe te voegen is nu de functie gebruikt met de volgende route:

    http://<yourapp>.azurewebsites.net/products/electronics/357

Zie voor meer informatie over andere eigenschappen die u in het bestand *host.json configureren kunt* [host.json verwijzing](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).





## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Testen van een functie](functions-test-a-function.md)
* [Schaal van een functie](functions-scale.md)
