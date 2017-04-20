<properties
   pageTitle="Betrouwbare communicatie-overzicht | Microsoft Azure"
   description="Overzicht van Services voor betrouwbare communicatiemodel, waaronder listeners op services openen, het oplossen van eindpunten en communicatie tussen services."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="how-to-use-the-reliable-services-communication-apis"></a>Het gebruik van de Services van betrouwbare communicatie API 's

Azure Service-structuur als een platform is volledig agnostische over de communicatie tussen services. Alle protocollen en stapels van UDP HTTP aanvaardbaar kunnen zijn. Is het aan de ontwikkelaar om te kiezen hoe services moeten communiceren. Betrouwbare Services application framework biedt ingebouwde communicatie stapels en API's waarmee u kunt bouwen uw aangepaste communicatie-onderdelen. 

## <a name="set-up-service-communication"></a>Service-communicatie

De betrouwbare Services API gebruikt een eenvoudige interface voor de communicatie van de service. U opent een eindpunt voor uw service, gewoon implementeert deze interface:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Vervolgens kunt u uw implementatie communicatie listener toevoegen door deze in een klasse service gebaseerde methode overschrijven.

Voor stateless diensten:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Voor stateful diensten:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

In beide gevallen moet u een collectie van listeners terug. Hierdoor kan de service voor meerdere endpoints, mogelijk met verschillende protocollen met meerdere listeners luisteren. Zoals wellicht u een HTTP-listener en een aparte WebSocket listener. Elke listener haalt een naam en de resulterende verzameling *naam: adres* paren als JSON-object wordt weergegeven wanneer een client hierom vraagt de luisterende adressen voor een service-exemplaar of een partitie.

De overschrijving in een stateless service geeft als resultaat een collectie van ServiceInstanceListeners. Een ServiceInstanceListener bevat een functie om een ICommunicationListener te maken en hieraan een naam. Stateful services, de overschrijving geeft als resultaat een collectie van ServiceReplicaListeners. Dit is iets anders dan de stateless tegenhanger, omdat een ServiceReplicaListener een optie heeft voor het openen van een ICommunicationListener op secundaire replica's. U kunt meerdere listeners voor communicatie in een service, niet alleen, maar u kunt ook opgeven welke listeners accepteren aanvragen op secundaire replica's en welke alleen op de primaire replica's luistert.

Zo hebt u een ServiceRemotingListener waarmee de RPC-aanroepen alleen op de primaire replica's en een tweede, aangepaste listener wordt gelezen op secundaire replica's via HTTP-aanvragen:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [AZURE.NOTE] Bij het maken van meerdere listeners voor een service, worden elke listener **moet** een unieke naam vermeld.

Ten slotte de eindpunten die vereist voor de service in het [manifest van de service](service-fabric-application-model.md) in de sectie op de eindpunten zijn wordt beschreven.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

De listener communicatie toegang tot het eindpunt bronnen toegewezen van de `CodePackageActivationContext` in de `ServiceContext`. De listener kunt start luisteren naar aanvragen wanneer het wordt geopend.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Eindpunt resources worden gebruikt in een pakket met de volledige service en ze door Fabric-Service worden toegewezen wanneer het servicepakket is geactiveerd. Meerdere ondergebracht in de ServiceHost dezelfde service replica's kunnen dezelfde poort delen. Dit betekent dat de listener communicatie poort delen moet ondersteunen. De aanbevolen manier is voor de communicatie een listener voor de partitie-ID en ID van de replica/exemplaar gebruiken wanneer het adres luisteren wordt gegenereerd.

### <a name="service-address-registration"></a>De service is geregistreerd adres

Een systeemservice die de *Naming Service* genoemd op clusters Fabric-Service wordt uitgevoerd. De Service voor naamgeving is een registrar voor services en de bijbehorende adressen die op elk exemplaar of de replica van de service luistert. Wanneer de `OpenAsync` methode van een `ICommunicationListener` is voltooid, de terugkeer waarde als resultaat gegeven in de Naming Service geregistreerd. Deze waarde die wordt gepubliceerd in de Service voor naamgeving is een tekenreeks waarvan de waarde van alles op alle zijn kan. Deze tekenreekswaarde is clients zien wanneer ze om een adres voor de service van de Service voor naamgeving vragen.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Fabric-service biedt een API's waarmee clients en andere services kunnen vragen voor dit adres door de naam van de service. Dit is belangrijk omdat het adres niet statisch is. Services worden verplaatst in het cluster voor taakverdeling en beschikbaarheid van resource. Dit is het mechanisme waarmee clients kunnen luisteren naar adres voor een service.

> [AZURE.NOTE] Voor een compleet overzicht van het schrijven van een `ICommunicationListener`, Zie [API-Fabric Web Service services met OWIN zelf hosten](service-fabric-reliable-services-communication-webapi.md)

## <a name="communicating-with-a-service"></a>Communicatie met een service
De betrouwbare Services API biedt de volgende bibliotheken om te schrijven van clients die met services communiceren.

### <a name="service-endpoint-resolution"></a>Service-eindpunt resolutie
De eerste stap bij de communicatie met een service is een adres van het eindpunt van de partitie of het exemplaar van de service die u wilt praten oplossen. De `ServicePartitionResolver` klasse utility is een eenvoudige primitieve die helpt bij het bepalen van het eindpunt van een service tijdens runtime clients. In Service Fabric-terminologie wordt het proces voor het bepalen van het eindpunt van een service van de *service endpoint resolutie*genoemd.

Verbinding maken met services in een cluster, `ServicePartitionResolver` kunnen worden gemaakt met de standaardinstellingen. Dit is de aanbevolen syntaxis voor de meeste situaties:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Verbinding maken met services in een ander cluster, een `ServicePartitionResolver` kan worden gemaakt met een cluster gateway eindpunten. Gateway-eindpunten zijn gewoon verschillende eindpunten voor de verbinding met het cluster dezelfde overigens. Bijvoorbeeld:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

U kunt ook `ServicePartitionResolver` een functie kan worden gegeven voor het maken van een `FabricClient` intern gebruik: 
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient`is het object dat wordt gebruikt om te communiceren met de Service Fabric '-cluster voor verschillende beheertaken uit te voeren op het cluster. Dit is handig als u meer controle over hoe u wilt `ServicePartitionResolver` communiceert met uw cluster. `FabricClient`caching intern uitgevoerd en is in het algemeen kostbaar om te maken, dus is het belangrijk om opnieuw te `FabricClient` zo veel mogelijk exemplaren. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Een methode resolve wordt vervolgens gebruikt voor het ophalen van het adres van een service of een partitie van de service voor gepartitioneerde services.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

Een adres kan worden opgelost met een `ServicePartitionResolver`, maar meer werk nodig is om het omgezette adres correct kan worden gebruikt. De client moet vaststellen of de verbindingspoging is mislukt vanwege een tijdelijke fout en opnieuw kan worden verzonden (bijvoorbeeld service verplaatst of tijdelijk niet beschikbaar is), of een permanente fout (bijvoorbeeld service is verwijderd of de aangevraagde bron bestaat niet meer). Replica's van exemplaren van de service of kunnen verplaatsen van knooppunt naar knooppunt op elk gewenst moment om meerdere redenen. Het adres van de omgezet via `ServicePartitionResolver` is mogelijk verlopen op het moment dat de clientcode probeert verbinding te maken. In dat geval opnieuw moet de client het adres te zetten. Met de vorige `ResolvedServicePartition` geeft aan dat de resolver nodig heeft om te proberen in plaats van gewoon ophalen een adres in de cache.

De clientcode hoeft meestal niet werken met de `ServicePartitionResolver` rechtstreeks. Het is gemaakt en doorgegeven aan communicatie client fabrieken in de betrouwbare Services API. De resolver wordt intern door de fabrieken gebruiken voor het genereren van een client-object dat kan worden gebruikt voor de communicatie met services.

### <a name="communication-clients-and-factories"></a>Communicatie-clients en fabrieken

De bibliotheek van de fabriek communicatie implementeert een typische afhandeling van fouten opnieuw patroon die bezig met opnieuw verbindingen met eindpunten opgelost gemakkelijker maakt. De bibliotheek van de fabriek is het mechanisme opnieuw terwijl u de fout-handlers.

`ICommunicationClientFactory`Hiermee definieert u de basisinterface geïmplementeerd door een fabriek van communicatie-client waarmee clients die naar een service Service Fabric kunnen praten. De uitvoering van de CommunicationClientFactory hangt af van de communicatie-stack gebruikt door de Service Fabric-service waar de klant wil communiceren. De betrouwbare Services API biedt een `CommunicationClientFactoryBase<TCommunicationClient>`. Dit zorgt voor een basis uitvoering van de `ICommunicationClientFactory` interface en taken die gemeenschappelijk voor alle communicatie stapels zijn uitgevoerd. (Deze taken omvatten het gebruik van een `ServicePartitionResolver` om te bepalen van het service-eindpunt). Clients worden meestal de abstracte klasse CommunicationClientFactoryBase voor het verwerken van de logica die specifiek is voor de communicatie-stack geïmplementeerd.

De communicatie-client alleen ontvangt een adres en wordt gebruikt voor verbinding met een service. De client kan ongeacht protocol deze wil gebruiken.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

De client-fabriek is primair verantwoordelijk is voor het maken van communicatie-clients. Voor clients die een permanente verbinding, zoals een HTTP-client niet onderhouden moet de fabriek alleen maken en de client terug. Andere protocollen die een permanente verbinding, zoals sommige binaire protocollen moeten ook worden gevalideerd door de fabriek om te bepalen of de verbinding moet opnieuw worden gemaakt.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Ten slotte is een uitzonderings-handler belast te bepalen welke actie moet worden ondernomen wanneer er een uitzondering optreedt. Uitzonderingen zijn gecategoriseerd in **retriable** en **niet retriable**. 

 - Uitzonderingen voor **retriable niet** ophalen gewoon opnieuw gegenereerd teruggegeven aan de aanroepfunctie. 
 - **Retriable** uitzonderingen kunnen verder worden onderverdeeld in **tijdelijke** en **tijdelijke**.
  - **Tijdelijke** uitzonderingen zijn die eenvoudig kunnen worden verwerkt zonder dat opnieuw het herleiden van het adres van het eindpunt. Hierbij gaat het tijdelijke netwerkproblemen of foutreacties van de service die aan dat het adres van het eindpunt bestaat niet. 
  - **Niet tijdelijk** uitzonderingen zijn waarvoor u het adres van het eindpunt worden weer opgelost. Dit zijn uitzonderingen die aangeven van dat het eindpunt van de service kan niet worden bereikt, is die aangeeft van de service verplaatst naar een ander knooppunt. 

De `TryHandleException` een besluit heeft genomen over een uitzondering gegeven. Als het **niet weet** wat besluiten over een uitzondering maken, moet deze uitvoer retourneren **de waarde false**. Als het **weet** wat besluit te maken, het moet het resultaat ook op Westers ingesteld en geeft **true**als resultaat.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>Alles samenbrengen
Met een `ICommunicationClient`, `ICommunicationClientFactory`, en `IExceptionHandler` gebouwd rond een communicatieprotocol, een `ServicePartitionClient` is het helemaal doorloopt en biedt de lus partitie fout afhandeling en service adres resolutie om deze onderdelen.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Volgende stappen
 - Zie een voorbeeld van HTTP-communicatie tussen services in een [voorbeeldproject op GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Externe procedure-oproepen met betrouwbare diensten remoting](service-fabric-reliable-services-communication-remoting.md)

 - [Web API die gebruikmaakt van OWIN in betrouwbare diensten](service-fabric-reliable-services-communication-webapi.md)

 - [WCF-communicatie via betrouwbare diensten](service-fabric-reliable-services-communication-wcf.md)
