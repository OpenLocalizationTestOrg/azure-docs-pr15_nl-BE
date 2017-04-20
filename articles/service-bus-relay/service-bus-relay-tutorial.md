<properties 
    pageTitle="Service Bus Relay-zelfstudie | Microsoft Azure"
    description="Maak een Bus Service client toepassing en Service Bus Relay-service."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-tutorial"></a>Service Bus Relay-zelfstudie

In deze zelfstudie wordt beschreven hoe u een eenvoudige Service Bus-clienttoepassing en de mogelijkheden van de 'relay' Service Bus-service maken. Zie voor een overeenkomstige zelfstudie [brokered messaging](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)Service Bus met de [Service Bus Brokered Messaging .NET zelfstudie](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Werken via deze zelfstudie hebt u een goed begrip van de stappen die nodig zijn om een toepassing voor client- en Service Bus. Net als hun tegenhangers WCF is een service een constructie die beschrijft een of meer eindpunten, die allemaal een of meer service-activiteiten. Het eindpunt van een service geeft een adres waar de service kan worden gevonden, een binding met de gegevens waarmee een client moet communiceren met de service en een contract waarin de functionaliteit van de service aan clients. Het belangrijkste verschil tussen een WCF en een Bus Service service is dat het eindpunt van de wolk in plaats van lokaal op uw computer is blootgesteld.

Nadat u de volgorde van de onderwerpen in deze zelfstudie doorloopt, hebt u een actieve service en een client die de activiteiten van de service kunt aanroepen. Het eerste onderwerp wordt beschreven hoe u een account instellen. De volgende stappen beschrijven het definiëren van een service die gebruikmaakt van een contract, het implementeren van de service en de service configureren in code. Ze manier waarop de service wordt uitgevoerd en wordt ook beschreven. De service die wordt gemaakt is zelfgehoste en de client en de service op dezelfde computer worden uitgevoerd. U kunt de service configureren met behulp van code of een configuratiebestand.

De laatste drie stappen wordt beschreven hoe u een clienttoepassing maken, de clienttoepassing configureren en maken en gebruiken van een client die toegang heeft tot de functionaliteit van de host.

Alle onderwerpen in dit gedeelte wordt ervan uitgegaan dat u als de ontwikkelomgeving Visual Studio gebruikt.

## <a name="sign-up-for-an-account"></a>Aanmelden voor een account

De eerste stap is een naamruimte maken en vervolgens een gedeeld Access handtekening (SAS) sleutel. Een naamruimte biedt een toepassingsgrenzen voor elke toepassing die toegankelijk zijn via de Service Bus. Een SAS-sleutel wordt automatisch gegenereerd door het systeem als een Servicenaamruimte wordt gemaakt. De combinatie van Servicenaamruimte en SAS-toets kunt u de referenties voor een Service Bus voor het verifiëren van een toepassing.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract-to-use-with-service-bus"></a>Een servicecontract WCF met Bus Service definiëren

Het servicecontract bepaalt welke bewerkingen de service ondersteunt (de Web service terminologie voor methoden of functies). Contracten worden gemaakt door het definiëren van een interface voor C++, C# of Visual Basic. Elke methode in de interface komt overeen met een specifieke bewerking. Elke interface moet het kenmerk [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) is toegepast, en elke bewerking moet hebben het kenmerk [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) is toegepast. Als een methode in een interface die het kenmerk [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) is niet het kenmerk [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) , wordt deze methode niet beschikbaar. De code voor deze taken is beschikbaar in het voorbeeld van de procedure te volgen. Zie voor een grotere bespreking van contracten en diensten, [ontwerpen en implementeren van diensten](https://msdn.microsoft.com/library/ms729746.aspx) in de WCF-documentatie.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Voor het maken van een contract Service Bus met een interface

1. Visual Studio openen als beheerder met de rechtermuisknop op het programma in het menu **Start** en selecteer **Als administrator uitvoeren**.

2. Maak een nieuwe console application-project. Klik in het menu **bestand** en selecteert u **Nieuw**aan en klik vervolgens op **Project**. In het dialoogvenster **Nieuw Project** , klikt u op **Visual C#** (als **Visual C#** niet, kijk onder **Andere talen**). Klik op de sjabloon **Consoletoepassing** en geef deze de naam **EchoService**. Klik op **OK** om het project te maken.

    ![][2]

3. Installeer het pakket Service Bus NuGet. Dit pakket automatisch verwijzingen naar de Service Bus bibliotheken, alsmede de WCF- **traceerbron**toegevoegd. [Traceerbron](https://msdn.microsoft.com/library/system.servicemodel.aspx) is de naamruimte waarmee u via programmering toegang tot de basisfuncties van WCF. Bus service gebruikt veel van de objecten en kenmerken van WCF-contracten.

    Met de rechtermuisknop op de oplossing in de Solution Explorer en klik op **Beheren NuGet pakketten voor oplossing**. Klik op het tabblad **Bladeren** en zoeken naar `Microsoft Azure Service Bus`. Controleer of de naam van het project is geselecteerd in het vak **versie (s)** . Klik op **installeren**en accepteer de gebruiksvoorwaarden.

    ![][3]

3. Dubbelklik in de Solution Explorer op het bestand Program.cs om deze te openen in de editor als deze nog niet is geopend.

4. Voeg de volgende instructies gebruiken aan het begin van het bestand:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Wijzig de naamruimtenaam van de standaardnaam van de **EchoService** in **Microsoft.ServiceBus.Samples**.

    >[AZURE.IMPORTANT] In deze zelfstudie wordt de C#-naamruimte **Microsoft.ServiceBus.Samples**, wordt de naamruimte van het contracttype beheerd dat wordt gebruikt in het configuratiebestand in de stap van [de WCF-client configureren](#configure-the-wcf-client) . Kunt u een naamruimte die u wilt dat bij het samenstellen van dit monster; de zelfstudie werkt echter niet tenzij u de naamruimten van het contract wijzigen en service, in het configuratiebestand. De naamruimte die is opgegeven in het bestand App.config moet hetzelfde zijn als de opgegeven naamruimte in de C#-bestanden.

1. Direct na de `Microsoft.ServiceBus.Samples` naamruimtedeclaratie, maar binnen de naamruimte, definieert u een nieuwe interface met de naam `IEchoContract` en de `ServiceContractAttribute` aan de interface met de **http://samples.microsoft.com/ServiceModel/Relay/**de naamruimtewaarde van het kenmerk. De naamruimtewaarde verschilt van de naamruimte waarmee u binnen het bereik van uw code. In plaats daarvan wordt de naamruimtewaarde gebruikt als unieke identificatie voor dit contract. Expliciet opgeven van de naamruimte, voorkomt u dat de standaardwaarde voor de naamruimte worden toegevoegd aan de naam van het contract.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] De naamruimte van de service-overeenkomst bevat meestal een naamgevingsschema met versie-informatie. Met inbegrip van versie-informatie in de naamruimte service contract kan services voor het isoleren van belangrijke wijzigingen door een nieuw servicecontract met een nieuwe naamruimte te definiëren en weer op een nieuwe eindpunt. Op deze manier kunnen clients worden nog steeds het oude servicecontract gebruiken zonder dat moet worden bijgewerkt. Versie-informatie kan bestaan uit een datum of een build-nummer. Zie [Service versiebeheer](http://go.microsoft.com/fwlink/?LinkID=180498)voor meer informatie. Voor de toepassing van deze zelfstudie wordt bevat het schema voor naamgeving van de naamruimte service contract geen versie-informatie.

1. Binnen de `IEchoContract` interface, declareert u een methode voor één bewerking de `IEchoContract` contract beschrijft in de interface en de toepassing de `OperationContractAttribute` kenmerken van de methode die u wilt weergeven als onderdeel van de overheidsopdracht Service Bus.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Direct na de `IEchoContract` interface definitie, declareert u een kanaal dat gegevens van beide overneemt `IEchoContract` en zo ook de `IClientChannel` interface, zoals hier wordt weergegeven:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Een kanaal is de WCF-object waarmee de host en client informatie aan elkaar doorgeven. Later schrijft code ten opzichte van het kanaal echo tussen de twee toepassingen.

1. Klik op **Oplossing bouwen** of drukt u op **Ctrl + Shift + B** om de nauwkeurigheid van uw werk tot nu toe vanuit het menu **Build** .

### <a name="example"></a>Voorbeeld

De volgende code toont een eenvoudige interface die u een contract Service Bus definieert.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Nu dat de interface is gemaakt, kunt u de interface implementeren.

## <a name="implement-the-wcf-contract-to-use-service-bus"></a>Uitvoering van het contract WCF Service Bus gebruiken

Een relay Service Bus te maken, moet u eerst de overeenkomst, die is gedefinieerd met behulp van een interface. Zie voor meer informatie over het maken van de interface, de vorige stap. De volgende stap is voor het implementeren van de interface. Dit omvat het maken van een klasse met de naam `EchoService` die worden geïmplementeerd door de gebruiker gedefinieerde `IEchoContract` interface. Nadat u de interface implementeert, wordt de interface met een configuratiebestand App.config vervolgens configureren. Het configuratiebestand bevat de benodigde informatie voor de toepassing, zoals de naam van de service, de naam van het contract en het type protocol dat wordt gebruikt om te communiceren met de Service Bus. De code die wordt gebruikt voor deze taken is beschikbaar in het voorbeeld van de procedure te volgen. Zie voor een meer algemene discussie over het implementeren van een servicecontract [Uitvoering van opdrachten voor diensten](https://msdn.microsoft.com/library/ms733764.aspx) in de WCF-documentatie.

1. Maak een nieuwe klasse met de naam `EchoService` direct achter de definitie van de `IEchoContract` interface. De `EchoService` klasse implementeert de `IEchoContract` interface. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Net als bij andere implementaties interface, kunt u de definitie implementeren in een ander bestand. Echter de uitvoering van deze zelfstudie, bevindt zich in hetzelfde bestand als de interfacedefinitie van de en de `Main` methode.

1. Toepassing van het kenmerk [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) aan de `IEchoContract` interface. Het kenmerk bevat de naam van de service en een naamruimte. Nadat u dit doet, de `EchoService` klasse wordt als volgt weergegeven:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Implementeer de `Echo` methode die is gedefinieerd de `IEchoContract` interface in de `EchoService` klasse. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Klik op **maken**en klik vervolgens op **Oplossing bouwen** om te bevestigen de juistheid van uw werk.

### <a name="to-define-the-configuration-for-the-service-host"></a>De configuratie voor de ServiceHost definiëren

1. Het configuratiebestand is vergelijkbaar met een WCF-configuratiebestand. Bevat de naam van het eindpunt (dat wil zeggen, de locatie Service Bus beschrijft voor clients en hosts met elkaar te communiceren) en de binding (het type protocol dat wordt gebruikt om te communiceren). Het belangrijkste verschil is dat deze geconfigureerde service-eindpunt verwijst naar een binding [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) , die geen deel uitmaakt van .NET Framework. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) is een van de bindingen die zijn gedefinieerd door de Service Bus.

1. Dubbelklik op het bestand App.config om deze te openen in de editor van Visual Studio **Solution Explorer**.

2. In de `<appSettings>` -element, vervangen de tijdelijke aanduidingen door de naam van de naamruimte van de service en de SA's van de sleutel die u hebt gekopieerd in een eerdere stap. 

1. In de `<system.serviceModel>` -tags toevoegen een `<services>` element. U kunt meerdere Service Bus toepassingen definiëren in een enkel configuratiebestand. In deze zelfstudie wordt echter slechts één.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. In de `<services>` -element toevoegen een `<service>` element definieert de naam van de service.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. In de `<service>` -element definieert de locatie van het eindpuntcontract en het type van de binding voor het eindpunt.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Het eindpunt wordt gedefinieerd wanneer de client zoekt naar de hosttoepassing. De zelfstudie wordt later deze stap voor het maken van een URI die de host via Bus-Service ondersteunt. De binding verklaart dat we TCP protocol gebruiken om te communiceren met de Service Bus.

1. Klik in het menu **Build** **Oplossing bouwen** om te bevestigen de juistheid van uw werk.

### <a name="example"></a>Voorbeeld

De volgende code ziet u de implementatie van het servicecontract.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

De volgende code toont de basisindeling van het App.config bestand dat is gekoppeld aan de ServiceHost.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-service-bus"></a>Host en uitvoeren van een eenvoudige webservice registreren bij Service Bus

Deze stap wordt beschreven hoe u een eenvoudige Service Bus-service uit te voeren.

### <a name="to-create-the-service-bus-credentials"></a>Referenties voor het maken van de Bus-Service

1. In `Main()`, maakt u twee variabelen die voor het opslaan van de naamruimte en de SA's die sleutel van het consolevenster worden gelezen.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    De SAS-sleutel worden later gebruikt voor toegang tot uw project Service Bus. De naamruimte is als een parameter doorgegeven aan `CreateServiceUri` voor het maken van een service-URI.

4. Met een [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) -object declareren dat u van een SAS-sleutel als het referentietype gebruikmaakt. Voeg de volgende code direct na de code toegevoegd in de laatste stap.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>Voor het maken van een basisadres voor de service

1. Na de code die u hebt toegevoegd in de laatste stap maken een `Uri` instantie voor het basisadres van de service. Deze URI bevat de Service Bus schema, de naamruimte en het pad van de interface.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    'sb' is een afkorting voor de Service Bus schema en geeft aan dat we het protocol TCP gebruiken. Dit is ook eerder aangegeven in het configuratiebestand als [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) is opgegeven als de binding.
    
    Voor deze zelfstudie, de URI is `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="to-create-and-configure-the-service-host"></a>Maken en de ServiceHost configureren

1. De modus verbinding instelt op `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Beschrijving van de modus verbinding het protocol dat de service wordt gebruikt om te communiceren met de Service Bus; HTTP- of TCP. Met de standaardinstelling `AutoDetect`, probeert de service verbinding maken met Bus-Service via TCP-als deze beschikbaar is en HTTP als TCP niet beschikbaar is. Opmerking dat dit van het protocol de service verschilt geeft voor clientcommunicatie. Dit protocol wordt bepaald door de binding gebruikt. Een service kan bijvoorbeeld de binding [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) , waarmee het eindpunt (beschikbaar op Service Bus) communiceren met clients via HTTP te gebruiken. Die dezelfde service kan **ConnectivityMode.AutoDetect** opgeven, zodat de service met de Bus-Service via TCP communiceert.

1. Maken van de servicehost, de URI die eerder in deze sectie hebt gemaakt.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    De ServiceHost is het WCF-object dat een exemplaar maakt van de service. Hier, als u gebruikmaakt van het type service dat u wilt maken (een `EchoService` type), en ook het adres waar u wilt tonen van de service.

1. Toevoegen aan de bovenkant van het bestand Program.cs en verwijzingen naar [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) en [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx).

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. In `Main()`, het eindpunt voor openbare toegang configureren.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    In deze stap vertelt Service Bus die uw toepassing openbaar kan worden gevonden door het onderzoeken van de Service Bus ATOM-feed voor uw project. Als u in een **particuliere** **DiscoveryType** , zou een client nog wel toegang tot de service. De service zou niet weergegeven bij het zoeken van de naamruimte Service Bus. De client moet in plaats daarvan van tevoren weet of het pad van het eindpunt.

1. De service de referenties van toepassing op de service-endpoints in het bestand App.config gedefinieerd:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Zoals vermeld in de vorige stap, kunt u hebt gedeclareerd verschillende services en eindpunten in het configuratiebestand. Als u had, zou deze code via het configuratiebestand en het zoeken naar elk eindpunt waarop uw referenties moet toepassen. Voor deze zelfstudie, heeft het configuratiebestand slechts één eindpunt.

### <a name="to-open-the-service-host"></a>De ServiceHost openen

1. Open de service.

    ```
    host.Open();
    ```

1. Gemeld dat de service wordt uitgevoerd en wordt uitgelegd hoe u de service afsloot.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Wanneer u klaar bent, sluit u de ServiceHost.

    ```
    host.Close();
    ```

1. Druk op **Ctrl + Shift + B** om te bouwen van het project.

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld bevat het servicecontract en de implementatie van de vorige stappen in de zelfstudie en fungeert als host voor de service in een console-programma. Compileer de volgende in een uitvoerbaar bestand met de naam EchoService.exe.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Een WCF-client voor het servicecontract maken

De volgende stap is een eenvoudige Service Bus-clienttoepassing maken en definiëren van het servicecontract dat u in de volgende stappen implementeren wilt. Opmerking dat veel van deze stappen lijken op de stappen voor het maken van een service: definiëren van een contract, het bewerken van een App.config bestand, referenties met verbinding met de Service Bus, enzovoort. De code die wordt gebruikt voor deze taken is beschikbaar in het voorbeeld van de procedure te volgen.

1. Maak een nieuw project in de huidige Visual Studio-oplossing voor de client als volgt:
    1. In de Solution Explorer, in dezelfde oplossing met de service, klik met de rechtermuisknop op de huidige oplossing (niet in het project) en klikt u op **toevoegen**. Klik vervolgens op **Nieuw Project**.
    2. In het dialoogvenster **Nieuw Project toevoegen** , klikt u op **Visual C#** (als **Visual C#** niet, kijk onder **Andere talen**), selecteert u de sjabloon **Consoletoepassing** en geef deze de naam **EchoClient**.
    3. Klik op **OK**.
<br />

1. Dubbelklik in de Solution Explorer op het bestand Program.cs in het **EchoClient** -project te openen in de editor als deze nog niet is geopend.

1. Wijzig de naamruimtenaam van de standaardnaam van de `EchoClient` op `Microsoft.ServiceBus.Samples`.

1. Installeer het [pakket Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus). In de Solution Explorer met de rechtermuisknop op het **EchoClient** -project en klik vervolgens op **Beheren NuGet pakketten**. Klik op het tabblad **Bladeren** en zoeken naar `Microsoft Azure Service Bus`. Klik op **installeren**en accepteer de gebruiksvoorwaarden.

    ![][3]

1. Voeg toe een `using` -instructie voor de naamruimte [traceerbron](https://msdn.microsoft.com/library/system.servicemodel.aspx) in het bestand Program.cs. 

    ```
    using System.ServiceModel;
    ```

1. Definitie van het contract toevoegen aan de naamruimte, zoals in het volgende voorbeeld. Houd er rekening mee dat deze definitie identiek aan de definitie in het project **Service** gebruikt is. Deze code toe te voegen aan de bovenkant van de `Microsoft.ServiceBus.Samples` naamruimte.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Druk op **Ctrl + Shift + B** om de client te bouwen.

### <a name="example"></a>Voorbeeld

De volgende code toont de huidige status van het bestand Program.cs in het EchoClient-project.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>De WCF-client configureren

In deze stap maakt u een App.config bestand voor de toepassing van een basis-client die toegang heeft tot de service die eerder in deze handleiding hebt gemaakt. Dit bestand App.config definieert het contract, de binding en de naam van het eindpunt. De code die wordt gebruikt voor deze taken is beschikbaar in het voorbeeld van de procedure te volgen.

1. Dubbelklik op **App.config** om het te openen in de Visual Studio-editor in het **EchoClient** -project in de Solution Explorer.

2. In de `<appSettings>` -element, vervangen de tijdelijke aanduidingen door de naam van de naamruimte van de service en de SA's van de sleutel die u hebt gekopieerd in een eerdere stap.

1. Toevoegen in het element traceerbron een `<client>` element.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Deze stap verklaart dat u een clienttoepassing WCF-stijl definieert.

1. In de `client` -element definieert de naam, het contract en het bindingstype voor het eindpunt.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    In deze stap definieert de naam van het het contract dat is gedefinieerd in de service en het feit dat de clienttoepassing gebruikmaakt van TCP om te communiceren met de Bus-Service-eindpunt. De naam van het wordt gebruikt in de volgende stap koppelt u deze eindpuntconfiguratie met de service-URI.

1. Klik op **bestand**en **opslaan van alle**.

## <a name="example"></a>Voorbeeld

De volgende code toont het App.config bestand voor de Echo-client.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client-to-call-service-bus"></a>Implementeren van de client voor WCF Bus Service call

In deze stap implementeert u een eenvoudige toepassing die toegang heeft tot de service die u eerder hebt gemaakt in deze zelfstudie. Vergelijkbaar met de service, voert de client veel dezelfde bewerkingen voor toegang tot de Service Bus:

1. De modus verbinding instellen.
1. Hiermee maakt u de URI die de hostservice wordt gezocht.
1. Hiermee definieert u de beveiligingsreferenties.
1. Geldt de referenties voor de verbinding.
1. De verbinding wordt geopend.
1. De specifieke taken uitvoert.
1. Hiermee sluit u de verbinding.

Een van de belangrijkste verschillen is echter dat de clienttoepassing een kanaal gebruikt voor verbinding met de Service Bus, dat de service wordt uitgevoerd een aanroep naar de **ServiceHost**. De code die wordt gebruikt voor deze taken is beschikbaar in het voorbeeld van de procedure te volgen.

### <a name="to-implement-a-client-application"></a>Voor het implementeren van een clienttoepassing

1. De modus verbinding ingesteld op **Automatische detectie**. Voeg de volgende code in de `Main()` methode van de toepassing van de **EchoClient** .

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Variabelen bevatten de waarden die zijn gelezen uit de console voor de naamruimte van service en SAS sleutel definiëren.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. De URI die de locatie van de host in uw project Service Bus definieert maken.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Het object referentie voor uw service-eindpunt naamruimte maken.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Maak de fabriek kanaal dat de configuratie die wordt beschreven in het bestand App.config wordt geladen.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Kanaal-factory is een WCF-object dat wordt gemaakt van een kanaal waarlangs de service en client-toepassingen communiceren.

1. De Bus Service referenties van toepassing.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Maken en het kanaal om de service te openen.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. De eenvoudige interface en de functionaliteit voor de echo schrijven.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Houd er rekening mee dat de code maakt gebruik van het exemplaar van het object kanaal als proxy voor de service.

1. Sluit het kanaal en de fabriek te sluiten.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>De toepassingen kunnen uitvoeren

1. Druk op **Ctrl + Shift + B** om de oplossing te bouwen. Dit maakt zowel de client-project als het project service die u in de vorige stappen hebt gemaakt.

2. Voordat de clienttoepassing wordt uitgevoerd, moet u ervoor zorgen dat de service-toepassing wordt uitgevoerd. Klik met de rechtermuisknop op de **EchoService** -oplossing in Visual Studio Solution Explorer en klik op **Eigenschappen**.

3. In het dialoogvenster Eigenschappen van de oplossing **Project opstarten**, klik op de knop **meerdere projecten voor opstarten** . Zorg ervoor dat **EchoService** eerste wordt weergegeven in de lijst. 

4. Het vak **actie** voor de **EchoService** en de **EchoClient** projecten instellen te **starten**.

    ![][5]

5. Klik op **Projectafhankelijkheden**. Selecteer in het vak **projecten** **EchoClient**. Controleer of **dat echoservice** is ingeschakeld in het vak **Depends.exe op** .

    ![][6]

6. Klik op **OK** om het dialoogvenster **Eigenschappen** te sluiten.

7. Druk op **F5** voor het uitvoeren van beide projecten.

8. Zowel windows console opent en u vragen om de naamruimtenaam van de. De service moet eerst, dus voer de naamruimte in het consolevenster van de **EchoService** en druk op **Enter**.

9. Vervolgens wordt u gevraagd uw sleutel SAS. De SAS-toets en op ENTER drukt.

    Hier volgt een voorbeeld van de uitvoer van het consolevenster. Houd er rekening mee dat de waarden die hier zijn bijvoorbeeld uitsluitend.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    De service-toepassing worden afgedrukt naar het consolevenster het adres waarop luistert, zoals in het volgende voorbeeld.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. Geef dezelfde gegevens die u eerder hebt ingevoerd voor de servicetoepassing in het consolevenster van de **EchoClient** . Volg de vorige stappen als u de naamruimte van dezelfde service en SAS-sleutelwaarden voor de clienttoepassing.

11. Na het invoeren van deze waarden, de client een kanaal geopend naar de service en vraagt u om de tekst zoals weergegeven in het volgende voorbeeld van de console-uitvoer opgeven.

    `Enter text to echo (or [Enter] to exit):` 

    Typ wat tekst verzenden naar de service-toepassing en drukt u op Enter. Deze tekst wordt verzonden naar de service via de Echo service-bewerking en wordt weergegeven in het consolevenster van de service zoals in het volgende voorbeeld van de uitvoer.

    `Echoing: My sample text`

    De clienttoepassing ontvangt de geretourneerde waarde van de `Echo` bewerking is de oorspronkelijke tekst en deze wordt afgedrukt aan het consolevenster. Hier volgt een voorbeeld van de uitvoer van het clientvenster-console.

    `Server echoed: My sample text`

12. U kunt doorgaan met het verzenden van tekstberichten vanaf de client naar de service op deze manier. Wanneer u klaar bent, drukt u op Enter in de console-client en de service beide toepassingen beëindigen.

## <a name="example"></a>Voorbeeld

In het volgende voorbeeld ziet u het maken van een clienttoepassing, hoe de werkzaamheden van de service en het sluiten van de client als de bewerkingsaanroep is voltooid.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie bleek het bouwen van een client Service Bus toepassing en service met behulp van de Service Bus 'relay'-mogelijkheden. Zie voor een soortgelijke zelfstudie die Service Bus [Messaging](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)gebruikt, de [Service Bus Brokered Messaging .NET zelfstudie](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Zie de volgende onderwerpen voor meer informatie over Service Bus.

- [Messaging service Bus-overzicht](../service-bus-messaging/service-bus-messaging-overview.md)
- [Grondbeginselen van Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Service Bus-architectuur](../service-bus-messaging/service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
