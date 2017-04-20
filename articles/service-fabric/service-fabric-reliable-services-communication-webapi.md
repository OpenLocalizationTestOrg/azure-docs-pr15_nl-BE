<properties
   pageTitle="Communicatie met de API voor ASP.NET Web Service | Microsoft Azure"
   description="Informatie over het service-communicatie stapsgewijs implementeren met behulp van de ASP.NET-Web-API met de OWIN zelf hosten in de betrouwbare Services API."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Aan de slag: Service Fabric Web API-services met de OWIN zelf hosten

Azure Service Fabric brengt de mogelijkheden in uw handen wanneer bent u beslissen hoe u wilt dat uw diensten om te communiceren met gebruikers en met elkaar. Deze zelfstudie ligt de nadruk op de communicatie tussen ASP.NET Web API voor .NET (OWIN) zelf hosten in Service-Fabric betrouwbare Services API met geopende Web-Interface implementeren. We zullen dieper diep de betrouwbare diensten pluggable communicatie API. We zult Web API ook gebruiken in een geïllustreerd voorbeeld aan hoe u een aangepaste communicatie listener instellen.


## <a name="introduction-to-web-api-in-service-fabric"></a>Inleiding tot het Web API Service stof

ASP.NET Web API is een populaire en krachtige raamwerk voor het bouwen van HTTP-APIs op het .NET Framework. Als u nog niet vertrouwd bent met het framework Zie [aan de slag met ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) voor meer informatie.

Web API Service stof is de dezelfde ASP.NET Web API u kent en liefde. Het verschil is in hoe u *host* een toepassing Web API. Won't u Microsoft Internet Information Services (IIS). Voor een beter begrip van het verschil, laten we dit opsplitsen in twee delen:

 1. De Web-API-toepassing (met inbegrip van de controllers en modellen)
 2. De host (de webserver, meestal IIS)

Een Web API-toepassing zelf wordt niet gewijzigd. Het verschilt niet van Web API-toepassingen die u hebt gemaakt in het verleden en u zou moeten kunnen gewoon de meeste van de code van uw toepassing plaatst. Maar als u hebt hosting van IIS, waar u de toepassing host mogelijk iets anders dan wat u hebt gebruikt. Voordat we hier aan de hosting, laten we beginnen met iets meer vertrouwde: de toepassing van de Web-API.


## <a name="create-the-application"></a>De toepassing maken

Start een nieuwe Service Fabric-toepassing met een enkele stateless service in Visual Studio 2015 maken:

![Een nieuwe Service Fabric-toepassing maken](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

Een Visual Studio-sjabloon voor een stateless service Web-API gebruiken is voor u beschikbaar. In deze zelfstudie, zullen wij bouwen een project Web API maken die leidt tot wat u krijgen zou als u deze sjabloon hebt geselecteerd.

Selecteer een leeg project Stateless Service voor meer informatie over het bouwen van een Web API-project maken of u kunt starten met de sjabloon stateless service Web API en voert u langs.  

![Maak een stateless service](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

De eerste stap is het in sommige pakketten NuGet ophalen voor Web-API. Het pakket dat we willen gebruiken is Microsoft.AspNet.WebApi.OwinSelfHost. Dit pakket bevat alle benodigde pakketten voor Web-API en de *host* -pakketten. Dit is belangrijk hoger.

![Web API maakt met behulp van de NuGet Package Manager](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

Nadat de pakketten zijn geïnstalleerd, kunt u het gebouw van de basisstructuur van een Web API project beginnen. Als u Web API hebt gebruikt, de projectstructuur zien er zeer bekend. Voeg eerst een `Controllers` directory en een controller voor eenvoudige waarden:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;
    
namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

Vervolgens voegt u een klasse starten in de hoofdmap van het project voor het registreren van het bewerkingsplan, formatters en andere configuratie-instellingen. Dit is ook waar Web API op aansluit op de *host*, die later worden herzien. 

**Startup.cs**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

Dat is voor de toepassing. Op dit moment hebt u stelt alleen de basic Web API project lay-out. Tot nu toe niet moet het er niet veel anders uit Web API-projecten die u hebt gemaakt in het verleden of de standaardsjabloon voor Web-API. Uw bedrijfslogica gaat zoals gebruikelijk in de controllers en modellen.

Nu wat we doen over het hosten zodat we daadwerkelijk kunnen uitvoeren?

## <a name="service-hosting"></a>Hosting service

Stof Service, de service wordt uitgevoerd in een *serviceproces host*, een uitvoerbaar bestand dat de servicecode wordt uitgevoerd. Wanneer u een service met de betrouwbare Services API schrijft, worden uw project service alleen naar een uitvoerbaar bestand dat het servicetype registreert en wordt de code uitgevoerd. Dit geldt in de meeste gevallen bij het schrijven van een service op Fabric in .NET Service. Als u Program.cs in het project stateless service opent, weergegeven:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Als die eruit ziet verdacht gedraagt het ingangspunt voor een console-programma, dat is omdat het is.

Meer details over de service hostproces en de serviceregistratie vallen buiten het bestek van dit artikel. Maar het is belangrijk dat u weet nu dat *dat de servicecode wordt uitgevoerd in een eigen proces*.

## <a name="self-host-web-api-with-an-owin-host"></a>Zelf host Web API met een host OWIN

Gezien het feit dat de toepassingscode voor Web-API wordt gehost in een eigen proces, hoe u wijzen op een webserver? [OWIN](http://owin.org/)invoeren. OWIN is simpelweg een contract tussen .NET webtoepassingen en webservers. Traditioneel als ASP.NET (tot 5 MVC) wordt gebruikt, is de webtoepassing nauw gekoppeld aan IIS via System.Web. Web-API implementeert, OWIN, zodat kunt u een webtoepassing die wordt ontkoppeld van de webserver die als host fungeert. Hierdoor kunt u *zelf gehost* OWIN webserver die u in uw eigen proces kan starten. Dit past perfect bij de Service Fabric hostingmodel die hierboven beschreven.

In dit artikel gebruiken we Katana als OWIN host voor de toepassing van de Web-API. Katana is een open source OWIN host implementatie die is gebaseerd op [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) en de [HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)van Windows.

> [AZURE.NOTE] Voor meer informatie over de Katana, gaat u naar de [site van Katana](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Zie [OWIN 2 Self-Host ASP.NET Web-API gebruiken](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api)voor een snel overzicht van het gebruik van Katana voor het hosten van Web API zelf.


## <a name="set-up-the-web-server"></a>De webserver instellen

De betrouwbare Services API biedt een communicatie van binnenkomst waar u communicatie stapels waarmee gebruikers en clients die verbinding maken met de service kunt aansluiten:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

De web-server (en andere communicatie-stack met u in de toekomst, zoals WebSockets) moeten de interface ICommunicationListener integreren goed met het systeem gebruiken. De redenen daarvoor worden duidelijk in de volgende stappen.

Maak eerst een klasse met de naam OwinCommunicationListener dat ICommunicationListener implementeert:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

De ICommunicationListener-interface biedt drie methoden voor het beheer van een communicatie-listener voor uw service:

 - *OpenAsync*. Beginnen met luisteren naar aanvragen.
 - *CloseAsync*. Stoppen met luisteren naar aanvragen, verzoeken tijdens een vlucht te voltooien en afsluiten.
 - *Afbreken*. Alles annuleren en onmiddellijk stoppen.

Toevoegen om te beginnen persoonlijke klassenleden voor zaken die de listener moet functioneren. Deze wordt geïnitialiseerd door middel van de constructor en wordt later gebruikt bij het instellen van de luisterende URL.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }
   

    ...

```

## <a name="implement-openasync"></a>OpenAsync implementeren

Als u de webserver instelt, moet u twee soorten informatie:

 - Het *voorvoegsel van een URL-pad*. Dit is optioneel, is het goed voor u dit nu zo ingesteld dat u veilig meerdere webservices in uw toepassing hosten.
 - *Een poort*.

Voordat u een poort voor de webserver, is het belangrijk dat u begrijpt dat Fabric-Service een toepassingslaag die als buffer tussen uw toepassing en het onderliggende besturingssysteem biedt fungeert op wordt uitgevoerd. Als zodanig kunt Fabric-Service u de *eindpunten* voor uw services configureren. Fabric-service zorgt ervoor dat eindpunten beschikbaar voor uw service zijn te gebruiken. Op deze manier hoeft u te configureren ze zelf in het onderliggende OS omgeving. U kunt eenvoudig uw Fabric Service-toepassing in verschillende omgevingen zonder wijzigingen aanbrengt in uw toepassing hosten. (Bijvoorbeeld: u kunt hosten in Azure of in uw eigen datacenter dezelfde toepassing.)

Een eindpunt HTTP configureren in PackageRoot\ServiceManifest.xml:

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Deze stap is belangrijk, omdat het proces service host wordt uitgevoerd met beperkte referenties (netwerkservice van Windows). Dit betekent dat de service geen toegang hebt tot een HTTP-eindpunt instellen op zijn eigen. Met behulp van de eindpuntconfiguratie, weet Fabric-Service voor het instellen van de juiste toegangsbeheerlijst (ACL) voor de URL die de service luistert op. Fabric-service biedt ook een standaard plaats voor eindpunten configureren.


Terug in de OwinCommunicationListener.cs, kunt u beginnen met het implementeren van OpenAsync. Dit is waar u de webserver starten. Eerst de eindpunt-informatie en de URL die de service luistert op maken. De URL is afhankelijk van of de listener wordt gebruikt in een service stateless of stateful service. Voor een stateful-service moet de listener een uniek adres voor elke service stateful replica die luistert het programma op te maken. Het adres kan niet veel eenvoudiger voor stateless services. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }
    
    ...

```

Let op 'http://+' wordt hier gebruikt. Dit is om ervoor te zorgen dat de server op alle beschikbare adressen luistert, met inbegrip van localhost FQDN-naam en het IP van de machine.

De implementatie van OpenAsync is een van de belangrijkste redenen waarom de webserver (of een stapel communicatie) is geïmplementeerd als een ICommunicationListener in plaats van dat deze rechtstreeks vanuit openen `RunAsync()` in de service. De geretourneerde waarde van OpenAsync is het adres dat op de webserver luistert. Als dit adres aan het systeem wordt geretourneerd, wordt het het adres geregistreerd met de service. Fabric-service biedt een API waarmee clients en andere services kunnen vragen voor dit adres door de naam van de service. Dit is belangrijk omdat het adres niet statisch is. Services worden verplaatst in het cluster voor taakverdeling en beschikbaarheid van resource. Dit is het mechanisme waarmee clients kunnen luisteren naar adres voor een service.

Met dat in gedachten, OpenAsync de webserver wordt gestart en retourneert het adres waarop wordt geluisterd. Opmerking er geluisterd op 'http://+', maar voordat de OpenAsync resulteert in het adres, de "+" wordt vervangen door het IP- of de FQDN-naam van het knooppunt wordt momenteel op. Het adres dat wordt geretourneerd door deze methode is wat wordt geregistreerd in het systeem. Het is ook wat clients en andere services te zien als ze om het adres van een service vragen. Voor clients correct tot stand te brengen, moeten ze een werkelijke IP- of FQDN-naam in het adres.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Houd er rekening mee dat deze verwijst naar de Startup-klasse die is doorgegeven aan de OwinCommunicationListener in de constructor. Dit exemplaar is gestart wordt gebruikt door de webserver voor het bootstrappen van de toepassing van de Web-API.

De `ServiceEventSource.Current.Message()` regel wordt weergegeven in het venster diagnostische gebeurtenissen later, tijdens het uitvoeren van de toepassing om te bevestigen dat de server is gestart.

## <a name="implement-closeasync-and-abort"></a>CloseAsync doorvoeren en afbreken

Ten slotte, CloseAsync en afbreken als u wilt stoppen met de webserver worden geïmplementeerd. De webserver kan worden gestopt door het afstoten van de ingang van de server die is gemaakt tijdens de OpenAsync.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);
            
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);
    
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

In deze voorbeeldimplementatie zowel CloseAsync als afbreken gewoon de webserver stoppen. U kunt ervoor kiezen om een meer netjes gecoördineerde afsluiten van de webserver in CloseAsync. De afsluiting kan bijvoorbeeld tijdens een vlucht aanvragen moet worden voltooid voordat de terugkeer wacht.

## <a name="start-the-web-server"></a>Start de webserver

U bent nu klaar om te maken en terug te keren een exemplaar van de OwinCommunicationListener naar de webserver starten. Overschrijven in de serviceklasse (WebService.cs), de `CreateServiceInstanceListeners()` methode:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

Dit is waar de Web-API- *toepassing* en de OWIN *host* tot slot voldoen aan. De host (OwinCommunicationListener), krijgt een exemplaar van de *toepassing* (Web API) via de klasse opstarten. Fabric-service beheert vervolgens hun levenscyclus. Deze hetzelfde patroon kan meestal met een communicatie-stack worden gevolgd.

## <a name="put-it-all-together"></a>Alles tezamen

In dit voorbeeld niet hoeft te doen de `RunAsync()` methode, zodat die overschrijving kan eenvoudig worden verwijderd.

De uiteindelijke implementatie moet eenvoudig zijn. Alleen moeten worden gemaakt van de listener communicatie:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

De volledige `OwinCommunicationListener` klasse:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Nu dat u alle stukken op zijn plaats zet hebt, moet uw project een typische Web API-toepassing met ingangspunten betrouwbare Services API en een host OWIN eruit:


![Web API met betrouwbare Services API ingangspunten en de OWIN host](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## <a name="run-and-connect-through-a-web-browser"></a>Uitgevoerd en die verbinding maken via een webbrowser

Als u nog niet hebt gedaan, [uw ontwikkelomgeving instellen](service-fabric-get-started.md).


U kunt nu bouwen en implementeren van uw service. Druk op **F5** in Visual Studio kunnen bouwen en implementeren van de toepassing. In het venster diagnostische gebeurtenissen, wordt er een bericht dat aangeeft dat de server geopend op http://localhost:8281 /.


![Visual Studio diagnostische gebeurtenissen venster](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE] Als de poort al door een ander proces op uw computer is geopend, ziet u hier een fout. Hiermee wordt aangegeven dat de listener kan niet worden geopend. Als dit het geval is, probeer een andere poort voor de eindpuntconfiguratie in ServiceManifest.xml.


Zodra de service wordt uitgevoerd, opent u een browser en Ga naar de [api-http://localhost:8281/waarden](http://localhost:8281/api/values) te testen.

## <a name="scale-it-out"></a>Het schalen

Schalen van stateless web apps meestal betekent meer machines toe te voegen en draaien van de webtoepassingen op deze. Service-Fabric orchestration engine kan dit voor u doen wanneer er nieuwe knooppunten worden toegevoegd aan een cluster. Als u exemplaren van een staatloos service maakt, kunt u het aantal exemplaren dat u wilt maken. Fabric-service plaatst dat aantal exemplaren op de knooppunten in het cluster. En dat moet niet meer dan één exemplaar op elke één knooppunt te maken. U kunt ook opgeven dat Fabric-Service altijd een instantie op elk knooppunt maken door op te geven **-1** voor de telling van het exemplaar. Dit zorgt ervoor dat als u knooppunten voor het schalen van uw cluster toevoegt, een exemplaar van de staatloze service wordt gemaakt op de nieuwe knooppunten. Deze waarde is een eigenschap van het exemplaar van de service, zodat deze is ingesteld bij het maken van een service-exemplaar. U kunt dit doen via PowerShell:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

U kunt dit ook doen bij het definiëren van een standaardservice in een Visual Studio-project stateless service:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Zie [een toepassing distribueren](service-fabric-deploy-remove-applications.md)voor meer informatie over het maken van de toepassing en service-exemplaren.

## <a name="next-steps"></a>Volgende stappen

[Fouten opsporen in uw toepassing Fabric-Service met behulp van Visual Studio](service-fabric-debugging-your-application.md)
