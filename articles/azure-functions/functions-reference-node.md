<properties
    pageTitle="Azure NodeJS functies: referentie voor ontwikkelaars | Microsoft Azure"
    description="Het ontwikkelen van Azure functies met NodeJS begrijpen."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure functies, functies, verwerking van de gebeurtenis, webhooks, dynamische compute, zonder server-architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Azure NodeJS functies: referentie voor ontwikkelaars

> [AZURE.SELECTOR]
- [C#-script](../articles/azure-functions/functions-reference-csharp.md)
- [F # script](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

De ervaring van knooppunt/JavaScript voor Azure functies eenvoudig kunt exporteren van een functie die wordt doorgegeven een `context` -object voor het communiceren met de runtime en voor het ontvangen en verzenden van gegevens via de bindingen.

In dit artikel wordt ervan uitgegaan dat u de [Naslaginformatie voor ontwikkelaars van Azure functies](functions-reference.md)al hebt gelezen.

## <a name="exporting-a-function"></a>Exporteren van een functie

Alle JavaScript-functies moeten een enkel exporteren `function` via `module.exports` voor de runtime te zoeken of de functie uit te voeren. Deze functie omvat altijd een `context` object.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Bindingen van `direction === "in"` worden doorgegeven als functieargumenten, wat betekent dat u kunt gebruiken [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) voor het verwerken van dynamische nieuwe ingangen (bijvoorbeeld met behulp van `arguments.length` worden herhaald voor de "inputs"). Deze functionaliteit is erg handig als u slechts een trigger waarvoor geen extra ingangen zoals u voorspelbare toegang uw triggergegevens zonder te verwijzen naar tot de `context` object.

De argumenten worden altijd doorgegeven aan de functie in de volgorde waarin dat ze voorkomen in *function.json*, zelfs als u ze in de uitvoer-instructie niet opgeeft. Als u bijvoorbeeld `function(context, a, b)` en wijzig deze naar `function(context, a)`, u krijgt nog steeds de waarde van `b` in de functiecode door te verwijzen naar `arguments[3]`.

Alle bindingen, ongeacht de richting, ook worden doorgegeven op de `context` object (Zie hieronder). 

## <a name="context-object"></a>contextobject

De runtime gebruikt een `context` object doorgeven van gegevens naar en van uw functie en kunt u communiceren met de runtime.

Het contextobject is altijd de eerste parameter aan een functie en moet altijd worden opgenomen omdat deze methoden, zoals `context.done` en `context.log` die moet juist de runtime gebruiken. U kunt het object naam wat u maar wilt (dat wil zeggen `ctx` of `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>context.bindings

De `context.bindings` object alle binnenkomende en uitgaande gegevens worden verzameld. De gegevens worden toegevoegd op de `context.bindings` object via de `name` eigenschap van de binding. Bijvoorbeeld de volgende bindingsdefinitie in *function.json*gegeven, u hebt toegang tot de inhoud van de wachtrij via `context.bindings.myInput`. 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

De `context.done` functie leest de runtime dat u hebt uitgevoerd. Dit is belangrijk om aan te roepen wanneer u met de functie bent klaar. Als u niet de runtime toch nooit te weten dat de functie is voltooid. 

De `context.done` functie kunt u weer een door de gebruiker gedefinieerde fout doorgeven aan de runtime, als een eigenschap zak eigenschappen waarvan de eigenschappen worden overschreven op de `context.bindings` object.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>context.log(Message)

De `context.log` methode kunt u de uitvoer van log-instructies die aan elkaar zijn gekoppeld bij elkaar voor de registratie. Als u `console.log`, uw berichten wordt alleen weergegeven voor de proces niveau, die niet zo handig is.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

De `context.log` methode ondersteunt de parameter vorm die het knooppunt [util.format methode](https://nodejs.org/api/util.html#util_util_format_format) ondersteunt. Dus bijvoorbeeld code als volgt:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

kan als volgt worden geschreven:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>HTTP-triggers: context.req en context.res

In het geval van Triggers, HTTP, omdat deze een vast patroon te gebruiken `req` en `res` voor de HTTP-aanvraag en het antwoord-objecten die we besloten waarmee u gemakkelijk toegang tot die op de contextobject, in plaats van dat u de volledige `context.bindings.name` patroon.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Knooppunt versie & pakket Management

De versie van het knooppunt is momenteel vergrendeld op `5.9.1`. We zijn toe te voegen ondersteuning voor meer versies onderzoeken en zodat het kan worden geconfigureerd.

U kunt pakketten in uw functie opnemen door een *package.json* -bestand uploaden naar de map van uw functie in het bestandssysteem van de functie app. Bestand uploaden instructies, Zie de sectie **bijwerken functie app bestanden** van het [onderwerp ontwikkelaar functies Azure](functions-reference.md#fileupdate). 

U kunt ook `npm install` in de opdrachtregelinterface van de functie-app SCM (Kudu):

1. Ga naar: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klik op **Foutopsporingsconsole > CMD**.

3. Ga naar `D:\home\site\wwwroot\<function_name>`.

4. Uitvoeren van `npm install`.

Zodra de benodigde pakketten zijn geïnstalleerd, u deze importeren in uw functie op de gebruikelijke manier (dat wil zeggen via `require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>Omgevingsvariabelen

Als u een omgevingsvariabele of een waarde app, gebruik `process.env`, zoals in het volgende voorbeeld wordt getoond:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>Schrijfmachine CoffeeScript/ondersteuning

Er niet nog een rechtstreekse steun voor de auto-opstelling schrijfmachine/CoffeeScript via de runtime, zodat die alle zou moet worden behandeld buiten de runtime, bij de implementatie. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Azure functies: referentie voor ontwikkelaars](functions-reference.md)
* [Azure functies in C#: referentie voor ontwikkelaars](functions-reference-csharp.md)
* [Azure functies F #: referentie voor ontwikkelaars](functions-reference-fsharp.md)
* [Azure functies, triggers en bindingen](functions-triggers-bindings.md)
