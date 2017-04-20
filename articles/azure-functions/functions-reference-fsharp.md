<properties
    pageTitle="Azure functies F #: referentie voor ontwikkelaars | Microsoft Azure"
    description="Het ontwikkelen van Azure functies met F # begrijpen."
    services="functions"
    documentationCenter="fsharp"
    authors="sylvanc"
    manager="jbronsk"
    editor=""
    tags=""
    keywords="Azure functies, functies, verwerking, webhooks, dynamische compute, zonder server architecture, F, gebeurtenis#"/>

<tags
    ms.service="functions"
    ms.devlang="fsharp"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/09/2016"
    ms.author="syclebsc"/>

# <a name="azure-functions-f-developer-reference"></a>Azure functies F # Developer Reference

> [AZURE.SELECTOR]
- [C#-script](../articles/azure-functions/functions-reference-csharp.md)
- [F # script](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

F # voor Azure functies is een oplossing voor het uitvoeren van kleine stukjes code, of "functies", eenvoudig in de cloud. In de functie F # via functieargumenten overdrachten. Argumentnamen zijn opgegeven in `function.json`, en er zijn vooraf gedefinieerde namen voor toegang tot zaken als de functie logger en annulering van tokens.

In dit artikel wordt ervan uitgegaan dat u de [Naslaginformatie voor ontwikkelaars van Azure functies](functions-reference.md)al hebt gelezen.

## <a name="how-fsx-works"></a>De werking van .fsx

Een `.fsx` bestand is een script F #. Het kan worden beschouwd als een F #-project dat opgenomen in één bestand. Het bestand bevat code voor het programma (in dit geval de functie Azure) en richtlijnen voor het beheren van afhankelijkheden.

Als u werkt met een `.fsx` voor een functie Azure vaak vereiste assembly's automatisch voor u, zodat u zich kunt concentreren op de code van de functie in plaats van "standaardtekst" opgenomen.

## <a name="binding-to-arguments"></a>Binding met argumenten

Elke binding ondersteunt een reeks argumenten, zoals wordt beschreven in de [Azure functies, triggers en bindingen: referentie voor ontwikkelaars](functions-triggers-bindings.md). Een argument bindingen een trigger blob ondersteunt is bijvoorbeeld een POCO, die kan worden uitgedrukt met een F # record. Bijvoorbeeld:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

De functie F # Azure duurt een of meer argumenten. Wanneer we over Azure functies argumenten praten, verwijzen we naar _Invoerargumenten_ en _Uitvoerargumenten_ . Een argument input is precies wat het klinkt als: invoer voor de functie F # Azure. Een argument van de _uitvoer_ is veranderlijke gegevens of een `byref<>` argument dat fungeert als een manier om door te geven gegevens weer _uit_ van de functie.

In het voorbeeld hierboven, `blob` is een invoerargument en `output` is een argument voor de uitvoer. U ziet dat we gebruikt `byref<>` voor `output` (is niet nodig om toe te voegen de `[<Out>]` commentaar). Met behulp van een `byref<>` type kunt wijzigen welke record of een object dat het argument naar verwijst de functie.

Wanneer een record F # als een invoertype wordt gebruikt, moet de definitie van de record gemarkeerd met `[<CLIMutable>]` zodat het Azure functies kader om de velden op de juiste wijze ingesteld voordat de record wordt doorgegeven aan de functie. Onder de motorkap, `[<CLIMutable>]` setters voor de recordeigenschappen wordt gegenereerd. Bijvoorbeeld:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Een klasse F # kan ook worden gebruikt voor zowel in- en argumenten. Voor een klasse moet eigenschappen meestal getters en setters. Bijvoorbeeld:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Logboekregistratie

Uitvoer om aan te melden uw [streaming logboeken](../app-service-web/web-sites-streaming-logs-and-console.md) in F #, neemt de functie een argument van het type `TraceWriter`. Voor de consistentie wordt aangeraden dit argument de naam `log`. Bijvoorbeeld:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Asynchrone

De `async` workflow kan worden gebruikt, maar het resultaat moet geven een `Task`. U kunt dit doen met `Async.StartAsTask`, bijvoorbeeld:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Annulering-Token

Als de functie afsluiten zonder problemen te verwerken moet, u kunt daarvoor een [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument. Dit kan worden gecombineerd met `async`, bijvoorbeeld:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Importeren van naamruimten

Naamruimten kunnen worden geopend op de gebruikelijke manier:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

De volgende naamruimten worden automatisch geopend:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Verwijzen naar externe assembly 's

Op dezelfde manier framework-assembly verwijzingen worden toegevoegd met de `#r "AssemblyName"` richtlijn.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

De volgende assembly's worden automatisch door de hostomgeving Azure-functies toegevoegd:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Daarnaast de volgende verzamelingen zijn speciale geïntegreerd zijn en kan worden verwezen door de simplename (bv. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Als u verwijzen naar een privé-assembly wilt, kunt u uploaden van het assembly-bestand in een `bin` map ten opzichte van uw functie en de verwijzing naar het met behulp van het bestand (bijvoorbeeld een naam  `#r "MyAssembly.dll"`). Zie de volgende sectie op pakket-beheer voor meer informatie over het uploaden van bestanden naar de map van uw functie.

## <a name="editor-prelude"></a>Prelude-editor

Een editor die F #-Compiler Services ondersteunt worden niet op de hoogte van de naamruimten en assemblages die Azure functies worden automatisch opgenomen. Als zodanig, kan het handig zijn een prelude die helpt bij het vinden van de assembly's die u met editor, en expliciet naamruimten openen. Bijvoorbeeld:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Wanneer uw code wordt uitgevoerd in Azure functies, verwerkt de bron met `COMPILED` gedefinieerd, zodat de prelude editor wordt genegeerd.

## <a name="package-management"></a>Pakket management

Toevoegen als u NuGet pakketten in een functie F #, een `project.json` van het bestand in de map in het bestandssysteem van de toepassing van de functie van de functie. Hier volgt een voorbeeld van de `project.json` -bestand dat een NuGet package verwijzing toegevoegd naar `Microsoft.ProjectOxford.Face` versie 1.1.0:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Alleen de .NET Framework 4.6 wordt ondersteund, dus zorg ervoor dat uw `project.json` bestand `net46` zoals hier wordt weergegeven.

Wanneer u uploadt een `project.json` de runtime-bestand haalt de pakketten en verwijzingen voor het samenstellen van het pakket wordt automatisch toegevoegd. U hoeft niet te voegen `#r "AssemblyName"` richtlijnen. Alleen toe te voegen de vereiste `open` instructies voor uw `.fsx` bestand.

U kunt automatisch verwijzingen assembly's in uw editor prelude, voor het verbeteren van uw editor interactie met F # compileren Services plaatsen.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Het toevoegen van een `project.json` bestand aan uw Azure-functie

1. Begin door te controleren of uw app functie wordt uitgevoerd, die u kunt doen met de functie openen in de portal Azure. Dit ook geeft toegang tot de logboeken van streaming waar pakket installatie uitvoer wordt weergegeven.

2. Voor het uploaden van een `project.json` bestand, gebruikt u een van de methoden die worden beschreven in de [functie app bestanden bijwerken](functions-reference.md#fileupdate). Als u [Doorlopend implementatie voor Azure functies](functions-continuous-deployment.md)gebruikt, kunt u toevoegen een `project.json` bestand naar de staging vertakking te experimenteren met het voordat u deze toevoegt aan uw filiaal implementatie.

3. Na de `project.json` , ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld in de functie log de streaming bestand wordt toegevoegd:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Omgevingsvariabelen

Als u een omgevingsvariabele of een waarde app, gebruik `System.Environment.GetEnvironmentVariable`, bijvoorbeeld:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Hergebruik van code .fsx

U kunt de code uit andere `.fsx` bestanden door middel van een `#load` richtlijn. Bijvoorbeeld:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Paden biedt aan de `#load` richtlijn zijn van de locatie van de `.fsx` bestand.

* `#load "logger.fsx"`laadt een bestand in de map functie.

* `#load "package\logger.fsx"`laadt een bestand in de `package` map in de map functie.

* `#load "..\shared\mylogger.fsx"`laadt een bestand in de `shared` map op hetzelfde niveau als de functie, dat wil zeggen, direct onder de `wwwroot`.

De `#load` richtlijn werkt alleen met `.fsx` (F # script) bestanden, en niet met `.fs` bestanden.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [F # gids](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index)
* [Azure functies: referentie voor ontwikkelaars](functions-reference.md)
* [Azure functies in C#: referentie voor ontwikkelaars](functions-reference-csharp.md)
* [Azure NodeJS functies: referentie voor ontwikkelaars](functions-reference-node.md)
* [Azure functies, triggers en bindingen](functions-triggers-bindings.md)
* [Azure-functies testen](functions-test-a-function.md)
* [Azure functies schalen](functions-scale.md)
