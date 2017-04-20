<properties
    pageTitle="Azure bindingen voor HTTP-functies en webhook | Microsoft Azure"
    description="Het gebruik van HTTP- en webhook-triggers en bindingen in Azure functies begrijpen."
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
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure bindingen voor HTTP-functies en webhook

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe te configureren en triggers voor HTTP- en webhook en bindingen in Azure functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>Function.JSON voor bindingen van HTTP- en webhook

Het bestand *function.json* bevat de eigenschappen die betrekking op de aanvraag en het antwoord hebben.

Eigenschappen voor de HTTP-aanvraag:

- `name`: De naam variabele in de functiecode gebruikt voor het object request (of het hoofdgedeelte van de aanvraag voor Node.js functies).
- `type`: Het moet worden ingesteld op *httpTrigger*.
- `direction`: *In*moet worden ingesteld. 
- `webHookType`: Geldige waarden zijn voor triggers, WebHook, *github*, *vertraging*en *genericJson*. Voor een HTTP-trigger die niet van een WebHook, moet u deze eigenschap instelt op een lege tekenreeks. Zie de volgende sectie van [WebHook triggers](#webhook-triggers) voor meer informatie op WebHooks.
- `authLevel`: Niet van toepassing op WebHook-triggers. Ingesteld op "functie" moet de API sleutel 'anoniem' drop de API basisvereiste of 'admin' de hoofdsleutel API vereist. Zie [API sleutels](#apikeys) hieronder voor meer informatie.

Eigenschappen voor het HTTP-antwoord:

- `name`: De naam variabele in de functiecode wordt gebruikt voor het object response.
- `type`: *Http*moet worden ingesteld.
- `direction`: Moet zijn ingesteld op *uit*. 
 
Voorbeeld *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>WebHook-triggers

Een trigger WebHook is een HTTP-trigger die de volgende functies die zijn ontworpen voor WebHooks heeft:

* Voor bepaalde aanbieders van WebHook (momenteel GitHub en toegestane vertraging worden ondersteund), de runtime functies valideert de handtekening van de provider.
* De runtime functies biedt voor Node.js functies, in plaats van het object request het hoofdgedeelte van de aanvraag. Er is geen speciaal voor C#, omdat u bepalen wat wordt geleverd door de parametertype te geven. Als u `HttpRequestMessage` krijgt u het request-object. Als u een type POCO opgeeft, probeert de runtime functies parseren een JSON-object in het hoofdgedeelte van de aanvraag voor het vullen van de eigenschappen van het object.
* Functie van de HTTP-aanvraag moet een API-sleutel bevatten om een WebHook te activeren. Deze eis is facultatief voor WebHook HTTP-triggers.

Zie voor meer informatie over het instellen van een GitHub WebHook [GitHub Developer - WebHooks maken](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409).

## <a name="url-to-trigger-the-function"></a>URL van de functie activeren

Als u wilt een functie activeren, kunt u een HTTP-aanvraag verzenden naar een URL die een combinatie is van de functie app URL en naam van de functie:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>API-toetsen

Een API-sleutel moet worden opgenomen in een HTTP-aanvraag voor het starten van een HTTP- of WebHook-functie standaard. De sleutel kan worden opgenomen in een queryreeks-variabele met de naam `code`, of kunnen worden opgenomen in een `x-functions-key` HTTP-header. Voor niet-WebHook functies, kunt u aangeven dat er een API-sleutel is vereist door de `authLevel` eigenschap 'anoniem' in het bestand *function.json* .

Waarden van de API kunt u vinden in de map *D:\home\data\Functions\secrets* in het bestandssysteem van de functie app.  De kapitein en functie sleutel instellen in het bestand *host.json* zoals in het volgende voorbeeld. 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

De functietoets van *host.json* kan worden gebruikt voor het starten van een functie, maar een uitgeschakelde won't activeringsfunctie. De hoofdsleutel kan worden gebruikt voor het starten van een functie en een functie wordt geactiveerd, zelfs als deze uitgeschakeld. U kunt een functie als u wilt dat de hoofdsleutel door de `authLevel` eigenschap 'admin'. 

Als de map *geheimen* een JSON-bestand met dezelfde naam als een functie bevat, de `key` eigenschap in dat bestand kan ook worden gebruikt voor het starten van de functie en deze sleutel werkt alleen met de functie verwijst. Bijvoorbeeld, de API-sleutel voor een functie met de naam `HttpTrigger` in *HttpTrigger.json* in de map *geheimen* is opgegeven. Hier volgt een voorbeeld:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Wanneer u een trigger WebHook instelt, de hoofdsleutel niet delen met de WebHook-provider. Gebruik een sleutel die alleen werkt met de functie die de WebHook verwerkt.  De hoofdsleutel kan worden gebruikt voor het starten van een functie, zelfs functies uitgeschakeld.

## <a name="example-c-code-for-an-http-trigger-function"></a>Van de C#-voorbeeldcode voor de functie van een HTTP-trigger 

In de voorbeeldcode wordt gezocht naar een `name` parameter in de querytekenreeks of in het hoofdgedeelte van de HTTP-aanvraag.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>F # voorbeeldcode voor een trigger HTTP-functie

In de voorbeeldcode wordt gezocht naar een `name` parameter in de querytekenreeks of in het hoofdgedeelte van de HTTP-aanvraag.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

U moet een `project.json` bestand dat NuGet gebruikt om te verwijzen naar de `FSharp.Interop.Dynamic` en `Dynamitey` assembly's als volgt:

```json
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
```

Dit NuGet wordt gebruikt voor het ophalen van de afhankelijkheden en verwijzen ernaar in uw script.

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>Node.js voorbeeldcode voor een trigger HTTP-functie 

Deze voorbeeldcode wordt gezocht naar een `name` parameter in de querytekenreeks of in het hoofdgedeelte van de HTTP-aanvraag.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>Van de C#-voorbeeldcode voor een functie GitHub WebHook 

Deze voorbeeldcode registreert GitHub probleem opmerkingen.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>F # voorbeeldcode voor een functie van GitHub WebHook

Deze voorbeeldcode registreert GitHub probleem opmerkingen.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>Node.js voorbeeldcode voor een functie van GitHub WebHook 

Deze voorbeeldcode registreert GitHub probleem opmerkingen.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
