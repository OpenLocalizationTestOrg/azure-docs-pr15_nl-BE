<properties
    pageTitle="Azure functies: referentie voor ontwikkelaars | Microsoft Azure"
    description="Het ontwikkelen van Azure functies met C# begrijpen."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure functies, functies, verwerking van de gebeurtenis, webhooks, dynamische compute, zonder server-architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure functies in C#: referentie voor ontwikkelaars

> [AZURE.SELECTOR]
- [C#-script](../articles/azure-functions/functions-reference-csharp.md)
- [F # script](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)
 
De C#-ervaring voor Azure functies is gebaseerd op de Azure WebJobs SDK. Overdrachten in de C#-functie via de argumenten van de methode. Argumentnamen zijn opgegeven in `function.json`, en er zijn vooraf gedefinieerde namen voor toegang tot zaken als de functie logger en annulering van tokens.

In dit artikel wordt ervan uitgegaan dat u de [Naslaginformatie voor ontwikkelaars van Azure functies](functions-reference.md)al hebt gelezen.

## <a name="how-csx-works"></a>De werking van .csx

De `.csx` kan schrijven minder "standaardtekst" en zich concentreren op het schrijven alleen een C#-functie. Voor Azure-functies alleen u alle assembly-verwijzingen en naamruimten moet u zoals gewoonlijk boven, omhoog en in plaats van alles wat tekstterugloop in een naamruimte en klasse, maar kunt u uw `Run` methode. Als u moet alle klassen, bijvoorbeeld POCO om objecten te definiëren, kunt u een klasse binnen hetzelfde bestand opnemen.

## <a name="binding-to-arguments"></a>Binding met argumenten

De verschillende bindingen zijn gebonden aan een C#-functie via de `name` eigenschap in de configuratie van de *function.json* . Elke binding heeft een eigen ondersteunde typen die wordt beschreven per binding; een trigger blob kan bijvoorbeeld een tekenreeks, een POCO of diverse andere typen ondersteunen. U kunt het type die het beste aansluit bij uw behoefte. 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>Logboekregistratie

U kunt opnemen als u uitvoer wilt toevoegen aan uw streaming Logboeken in C#, een `TraceWriter` argument hebt opgegeven. Wij raden aan dat u deze de naam `log`. Wij raden u niet aan `Console.Write` in Azure functies.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Asynchrone

Als u een functie asynchrone, gebruikt u de `async` trefwoord en terug een `Task` object.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>Annulering-Token

In bepaalde gevallen moet u wellicht die gevoelig zijn voor wordt afgesloten. Het is altijd het beste Schrijf code die vastlopen kan verwerken, in gevallen waar u aanvragen voor het verwerken van systeem correct wordt afgesloten, definieert u een [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument hebt opgegeven.  A `CancellationToken` krijgt als het afsluiten van een host wordt geactiveerd. 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Importeren van naamruimten

Als u nodig hebt voor het importeren van naamruimten, kunt u doen dus normaal met de `using` component.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

De volgende naamruimten worden automatisch geïmporteerd en zijn daarom optioneel:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Verwijzen naar externe assembly 's

Voor het framework-assemblages, verwijzingen toevoegen met behulp van de `#r "AssemblyName"` richtlijn.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
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
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

Als u verwijzen naar een privé-assembly wilt, kunt u uploaden van het assembly-bestand in een `bin` map ten opzichte van uw functie en de verwijzing naar het met behulp van het bestand (bijvoorbeeld een naam  `#r "MyAssembly.dll"`). Zie de volgende sectie op pakket-beheer voor meer informatie over het uploaden van bestanden naar de map van uw functie.

## <a name="package-management"></a>Pakket management

Als u pakketten NuGet in C#-functie, een *project.json* bestand uploadt naar de map in het bestandssysteem van de toepassing van de functie van de functie. Hier volgt een voorbeeld van de *project.json* -bestand dat een verwijzing toegevoegd naar versie 1.1.0 Microsoft.ProjectOxford.Face:

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

Alleen de .NET Framework 4.6 wordt ondersteund, dus zorg ervoor dat het bestand *project.json* bevat `net46` zoals hier wordt weergegeven.

Als u een *project.json* -bestand uploadt, worden de runtime haalt de pakketten en verwijzingen voor het samenstellen van het pakket wordt automatisch toegevoegd. U hoeft niet te voegen `#r "AssemblyName"` richtlijnen. Alleen toe te voegen de vereiste `using` instructies naar het bestand *run.csx* naar de gegevenstypen die zijn gedefinieerd in de NuGet-pakketten.


### <a name="how-to-upload-a-projectjson-file"></a>Het uploaden van een bestand project.json

1. Begin door te controleren of uw app functie wordt uitgevoerd, die u kunt doen met de functie openen in de portal Azure. 

    Dit ook geeft toegang tot de logboeken van streaming waar pakket installatie uitvoer wordt weergegeven. 

2. Een project.json om bestand te uploaden, gebruikt u een van de methoden die worden beschreven in de sectie **bijwerken functie app bestanden** van het [onderwerp ontwikkelaar functies Azure](functions-reference.md#fileupdate). 

3. Nadat u het bestand *project.json* wordt geladen, ziet u uitvoer zoals in het volgende voorbeeld in de functie log de streaming:

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

Als u een omgevingsvariabele of een waarde app, gebruik `System.Environment.GetEnvironmentVariable`, zoals in het volgende voorbeeld wordt getoond:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Hergebruik van code .csx

U kunt klassen en methoden die zijn gedefinieerd in de bestanden van andere *.csx* in het bestand *run.csx* . Gebruik hiervoor `#load` richtlijnen in het bestand *run.csx* , zoals in het volgende voorbeeld wordt getoond.

Voorbeeld *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Voorbeeld *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

U kunt een relatief pad met de `#load` richtlijn:

* `#load "mylogger.csx"`laadt een bestand in de map functie.

* `#load "loadedfiles\mylogger.csx"`laadt een bestand in een map in de map functie.

* `#load "..\shared\mylogger.csx"`laadt een bestand in een map op hetzelfde niveau als de functie, dat wil zeggen, direct onder de *map wwwroot*.
 
De `#load` richtlijn werkt alleen met *.csx* (C# script)-bestanden, niet met *:. cs* -bestanden. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Azure functies: referentie voor ontwikkelaars](functions-reference.md)
* [Azure functies F #: referentie voor ontwikkelaars](functions-reference-fsharp.md)
* [Azure NodeJS functies: referentie voor ontwikkelaars](functions-reference-node.md)
* [Azure functies, triggers en bindingen](functions-triggers-bindings.md)

