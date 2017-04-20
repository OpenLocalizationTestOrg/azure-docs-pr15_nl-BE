<properties
    pageTitle="Azure functies DocumentDB bindingen | Microsoft Azure"
    description="Begrijpen hoe Azure DocumentDB bindingen in Azure functies."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
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
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-documentdb-bindings"></a>Azure functies DocumentDB bindingen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe te configureren en code Azure DocumentDB bindingen in Azure functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="docdbinput"></a>Azure DocumentDB invoer binding

Invoer bindingen kunnen laden van een document uit een collectie DocumentDB en doorgegeven rechtstreeks aan de binding. De document-id kan worden bepaald op basis van de trigger dat de functie aangeroepen. In een C#-functie, eventuele wijzigingen in de record automatisch wordt verzonden naar de collectie wanneer de functie met succes wordt afgesloten.

#### <a name="functionjson-for-documentdb-input-binding"></a>Function.JSON voor DocumentDB invoer binding

Het bestand *function.json* bevat de volgende eigenschappen:

- `name`: De naam variabele in de functiecode wordt gebruikt voor het document.
- `type`: moet zijn ingesteld op 'documentdb'.
- `databaseName`: De database met het document.
- `collectionName`: De collectie met het document.
- `id`: De Id van het document op te halen. Deze eigenschap bindingen vergelijkbaar met "{queueTrigger}" ondersteunt die de tekenreekswaarde van het bericht wachtrij wordt gebruikt als de id van het document
- `connection`: Deze tekenreeks moet een instelling van toepassing is ingesteld op het eindpunt voor de account DocumentDB. Als u uw account op het tabblad Integratie kiest, wordt de instelling van een nieuwe App voor u gemaakt met een naam die de volgende vorm, yourAccount_DOCUMENTDB. Als u de instelling van de App handmatig te maken, de werkelijke verbindingsreeks moet er als volgt uit, AccountEndpoint =<Endpoint for your account>; AccountKey =<Your primary access key>;.
- ' richting: moet zijn ingesteld op *"in"*.

Voorbeeld *function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB invoer voorbeeld voor C# queue triggers
 
Met het bovenstaande voorbeeld-function.json, de DocumentDB invoer binding het document met de id die overeenkomt met de tekenreeks wachtrij worden opgehaald en doorgegeven aan de parameter 'document'. Als dat document niet wordt gevonden, wordt de parameter 'document' niet null zijn. Het document wordt vervolgens bijgewerkt met de nieuwe tekstwaarde bij het verlaten van de functie.
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB invoer voorbeeld voor een wachtrij-trigger F #

Met het bovenstaande voorbeeld-function.json, de DocumentDB invoer binding het document met de id die overeenkomt met de tekenreeks wachtrij worden opgehaald en doorgegeven aan de parameter 'document'. Als dat document niet wordt gevonden, wordt de parameter 'document' niet null zijn. Het document wordt vervolgens bijgewerkt met de nieuwe tekstwaarde bij het verlaten van de functie.

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

U moet een `project.json` bestand dat NuGet gebruikt om aan te geven de `FSharp.Interop.Dynamic` en `Dynamitey` pakketten als pakketafhankelijkheden, als volgt:

    {
      "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
          }
        }
      }
    }

Dit NuGet wordt gebruikt voor het ophalen van de afhankelijkheden en verwijzen ernaar in uw script.

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB invoer voorbeeld voor een wachtrij Node.js trigger
 
Met het bovenstaande voorbeeld-function.json, de DocumentDB invoer binding wordt het document met de id die overeenkomt met de tekenreeks wachtrij ophalen en doorgeven aan de `documentIn` bindingseigenschap. In Node.js functies, worden bijgewerkte documenten niet verzonden naar de collectie. Echter kunt u doorgeven de invoer binding rechtstreeks aan de uitvoer van een DocumentDB binding met de naam `documentOut` updates ondersteunen. In dit voorbeeld de eigenschap text van het invoerdocument worden bijgewerkt en ingesteld als het uitvoerdocument.
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a id="docdboutput"></a>Azure DocumentDB uitvoer bindingen

Uw functies JSON kunnen schrijven documenten met een Azure-DocumentDB-database met behulp van de **Azure DocumentDB Document** output binding. Bekijk voor meer informatie over Azure DocumentDB de [Inleiding tot DocumentDB](../documentdb/documentdb-introduction.md) en de [zelfstudie aan de slag](../documentdb/documentdb-get-started.md).

#### <a name="functionjson-for-documentdb-output-binding"></a>Function.JSON voor DocumentDB uitvoer binding

Het bestand function.json bevat de volgende eigenschappen:

- `name`: De naam variabele in de functiecode voor het nieuwe document gebruikt.
- `type`: moet zijn ingesteld op *'documentdb'*.
- `databaseName`: De database met de collectie waar het nieuwe document wordt gemaakt.
- `collectionName`: De collectie waar het nieuwe document wordt gemaakt.
- `createIfNotExists`: Dit is een Boole-waarde om aan te geven of de collectie worden gemaakt als deze nog niet bestaat. De standaardwaarde is *false*. De reden voor dit nieuwe is collecties gemaakt met gereserveerde doorvoer die prijzen implicaties heeft. Voor meer informatie Ga naar de [pagina prijzen](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: Deze tekenreeks moet dat een **Toepassingsinstelling** ingesteld op het eindpunt voor de account DocumentDB. Als u uw account op het tabblad **integratie** , een nieuwe App-instelling wordt gemaakt voor u met een naam die de volgende vorm `yourAccount_DOCUMENTDB`. Als u de instelling van de App handmatig te maken, de werkelijke verbindingsreeks moet er als volgt uit, `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: moet zijn ingesteld op *"out"*. 
 
Voorbeeld function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB output voorbeeld voor een wachtrij Node.js trigger

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

Het uitvoerdocument:

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB output voorbeeld voor een wachtrij-trigger F #

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB output voorbeeld voor C# queue triggers


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure DocumentDB code voorbeeld Instellingsnaam uitvoerbestand

Als u wilt dat de naam van het document in de functie instellen, stelt de `id` waarde.  Bijvoorbeeld als de JSON-inhoud voor een werknemer is in de wachtrij met de volgende strekking wordt verbroken:

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

U kunt de volgende C#-code in een wachtrij trigger functie: 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

Of de gelijkwaardige F #-code:

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

Voorbeeld van de uitvoer:

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
