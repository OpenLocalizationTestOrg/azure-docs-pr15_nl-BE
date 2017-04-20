<properties
    pageTitle="Het gebruik van de relay Service Bus met .NET | Microsoft Azure"
    description="Leren werken met de Azure Service Bus relayservice verbinding maken met twee toepassingen die op verschillende locaties worden gehost."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# <a name="how-to-use-the-azure-service-bus-relay-service"></a>Het gebruik van de relay Azure Service Bus service

In dit artikel wordt beschreven hoe de Service Bus relay-service wilt gebruiken. De monsters zijn geschreven in C# en Windows Communication Foundation (WCF) API met extensies die zijn opgenomen in de assemblage Service Bus gebruiken. Zie voor meer informatie over de relay Service Bus overview [messaging Service Bus doorgegeven](service-bus-relay-overview.md) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-the-service-bus-relay"></a>Wat is de relay Service Bus?

De [Service Bus *relay* ](service-bus-relay-overview.md) -service kunt u bouwen hybride toepassingen die worden uitgevoerd in een datacenter Azure en uw eigen bedrijfsomgeving op gebouwen. De relay Service Bus vereenvoudigt dit doordat u services van Windows Communication Foundation (WCF) die zich binnen een netwerk van zakelijke onderneming met de openbare cloud, bevinden zonder dat een firewallverbinding te openen, of het vereisen van opdringerige wijzigingen in een bedrijfsnetwerkinfrastructuur veilig worden blootgesteld.

![Relay-concepten](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

De relay Service Bus kunt u host WCF-services binnen uw bestaande bedrijfsomgeving. U kunt overdragen luistert naar binnenkomende sessies en aanvragen naar deze WCF-services op de Service Bus-service wordt uitgevoerd in Azure. Hierdoor kunt u deze services toepassingscode uitgevoerd in Azure of mobiele werknemers of partner extranet omgevingen worden blootgesteld. Bus service kunt u veilig bepalen wie toegang deze services op een niveau toegesneden tot. Het biedt een krachtige en veilige manier om functies en gegevens uit uw bestaande onderneming oplossingen worden blootgesteld en profiteer van deze uit de cloud.

In dit artikel wordt beschreven hoe de relay Service Bus gebruikt voor het maken van een webservice WCF blootgesteld met behulp van een TCP-kanaal binding, die een veilige uitwisseling tussen twee partijen implementeert.

## <a name="create-a-service-namespace"></a>Een Servicenaamruimte maken

Om te beginnen met behulp van de relay Service Bus in Azure, moet u een naamruimte te maken. Een naamruimte biedt een scope-container voor het adresseren van Service Bus bronnen binnen uw toepassing.

Voor het maken van een Servicenaamruimte:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Het pakket Service Bus NuGet ophalen

De [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is de eenvoudigste manier om de Service Bus-API en voor het configureren van uw toepassing met alle afhankelijkheden Service Bus. De NuGet om pakket te installeren in uw project, het volgende doen:

1.  In Solution Explorer met de rechtermuisknop op **References**, klik op **Beheren NuGet pakketten**.
2.  'Service Bus' zoekt en selecteert u het item **Microsoft Azure Service Bus** . Klik op **installeren** om de installatie te voltooien en sluit vervolgens het volgende dialoogvenster:

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="use-service-bus-to-expose-and-consume-a-soap-web-service-with-tcp"></a>Service Bus te openbaren en verbruiken een SOAP-webservice met TCP gebruiken

Als u wilt weergeven op een bestaande WCF SOAP-webservice voor externe voeding, moet u wijzigingen aanbrengen in de servicebindingen en adressen. Hiervoor kunnen wijzigingen in uw configuratiebestand of het nodig kan worden gewijzigd van de code, afhankelijk van hoe u hebt ingesteld en geconfigureerd de WCF-services. Houd er rekening mee dat WCF u meerdere netwerkeindpunten via dezelfde service kunt, zodat u tijdens het toevoegen van eindpunten Bus Service voor externe toegang op hetzelfde moment kunt u de bestaande interne eindpunten behouden.

In deze taak u een eenvoudige WCF-service maken en een listener-Service Bus aan toe te voegen. In deze oefening wordt ervan uitgegaan dat enigszins bekend zijn met Visual Studio, en daarom wordt niet doorlopen de details van het maken van een project. In plaats daarvan de nadruk ligt op de code.

Voordat u deze stappen uitvoert, voert u de volgende procedure voor het instellen van uw omgeving:

1.  Maak een consoletoepassing die twee projecten 'Client' en 'Service', binnen de oplossing bevat binnen Visual Studio.
2.  Het pakket Microsoft Azure Service Bus NuGet toevoegen aan beide projecten. Dit pakket wordt het noodzakelijk een assembly-verwijzing aan uw projecten toegevoegd.

### <a name="how-to-create-the-service"></a>Het maken van de service

Maak eerst de service zelf. Een WCF-service bestaat uit ten minste drie onderdelen:

-   Definitie van een contract dat beschrijft welke berichten worden uitgewisseld en bewerkingen worden aangeroepen.
-   Implementatie van genoemde overeenkomst.
-   De host die fungeert als host voor de service WCF en beschrijft verschillende eindpunten.

De codevoorbeelden in deze sectie kunnen u elk van deze onderdelen.

Het contract bepaalt een enkele bewerking `AddNumbers`, die twee getallen worden opgeteld en wordt het resultaat. De `IProblemSolverChannel` interface kan de client de levensduur van de proxy eenvoudiger beheren. Maken van een dergelijke interface wordt wel aanbevolen. Het is verstandig deze definitie contract in een apart bestand zetten zodat u kunt verwijzen naar dat bestand uit de 'Client' en de 'Service'-projecten, maar u ook de code naar beide projecten kopiëren kunt.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Met het contract is de implementatie eenvoudig.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Via programmering een ServiceHost configureren

U kunt de service nu hosten met het contract en de uitvoering op zijn plaats. Hosting plaatsvindt binnen een [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx) -object, dat zorgt voor het beheren van exemplaren van de service en host is van de eindpunten die naar berichten luisteren. De volgende code wordt de service geconfigureerd met zowel een normale lokaal eindpunt en een Bus-Service-eindpunt ter illustratie van de weergave naast elkaar, van interne en externe eindpunten. Vervangen door de tekenreeks *naamruimte* de naamruimtenaam en de *yourKey* met SAS-sleutel die u hebt verkregen in de vorige stap van setup.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

In het voorbeeld maakt u twee eindpunten die op de uitvoering met dezelfde opdracht. Een lokale is en een geprojecteerd via Service Bus. De belangrijkste verschillen zijn de bindingen; [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) voor de lokale en [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) voor het Bus-Service-eindpunt en de adressen. Het lokale eindpunt heeft een lokale netwerkadres met een afzonderlijke poort. De Bus-Service-eindpunt is een adres van eindpunt bestaat uit de tekenreeks `sb`, de naam van de naamruimte en het pad 'Oplosser'. Dit resulteert in de URI `sb://[serviceNamespace].servicebus.windows.net/solver`, service-eindpunt verifiëren met een volledig gekwalificeerde externe DNS-naam of een Service Bus TCP-eindpunt. Als u de code vervangt de tijdelijke aanduidingen in de `Main` functie van de toepassing van de **Service** , hebt u een functionele service. Als u wilt dat de service uitsluitend op de Bus-Service luistert, verwijder de declaratie van het lokale endpoint.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>Een ServiceHost configureren in het bestand App.config

U kunt ook het bestand App.config van de host configureren. De code in dit geval hosting service wordt weergegeven in het volgende voorbeeld.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

De definities van het eindpunt verplaatsen naar het bestand App.config. Het pakket NuGet heeft al een reeks definities zijn uitbreidingen van de vereiste configuratie voor Service Bus App.config bestand toegevoegd. In het volgende voorbeeld, dat het exacte equivalent van de vorige code is, moet direct onder het element **traceerbron** verschijnen. In dit voorbeeld wordt ervan uitgegaan dat uw project C#-naamruimte is naam van **Service**.
De tijdelijke aanduidingen vervangen door uw Service Bus Servicenaamruimte en SAS-sleutel.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Nadat u deze wijzigingen aanbrengt, wordt de service gestart als voorheen, maar met twee live eindpunten: een lokale en een luisteren in de cloud.

### <a name="create-the-client"></a>De client maken

#### <a name="configure-a-client-programmatically"></a>Programmatisch een client configureren

De service in beslag neemt, kunt u een WCF-client met behulp van een [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) -object maken. Bus-service maakt gebruik van een op tokens gebaseerde beveiligingsmodel geïmplementeerd door middel van SAS. De klasse [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) geeft een beveiligingstokenprovider met fabriek ingebouwde methoden die resulteren in sommige providers voor bekende token. In het volgende voorbeeld wordt de methode [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) gebruikt voor het verwerken van het verkrijgen van de juiste SAS-token. De naam en sleutel worden verkregen via de portal, zoals beschreven in de vorige sectie.

Kopiëren of eerst verwijst naar de `IProblemSolver` code van de service in uw project client contract.

Vervang vervolgens de code in de `Main` methode van de client opnieuw de tijdelijke tekst vervangen door uw Service Bus naamruimte en SAS-sleutel.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

U kunt nu de client en de-service worden uitgevoerd (de service eerst uitgevoerd) bouwen en de client de service aanroept en **9**afgedrukt. U kunt de client en server op verschillende computers uitvoeren zelfs over een netwerk en de communicatie werkt nog steeds. De clientcode kan ook worden uitgevoerd in de cloud of lokaal.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Een client te configureren in het bestand App.config

De volgende code laat zien hoe de client met behulp van het bestand App.config configureren.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

De definities van het eindpunt verplaatsen naar het bestand App.config. Het volgende voorbeeld wordt hetzelfde als de eerder genoemde code is, moet direct onder het element **traceerbron** verschijnen. Hier, zoals voorheen, u moet de tijdelijke aanduidingen vervangen uw Service Bus naamruimte en SAS-sleutel.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van de relay Service Bus-service, klik op deze koppelingen voor meer informatie.

- [Bus service doorgegeven berichten overzicht](service-bus-relay-overview.md)
- [Overzicht van Azure Service Bus-architectuur](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- Download Service Bus monsters van [Azure monsters][] of Zie het [overzicht van de monsters Service Bus][].

  [Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
  [Azure monsters]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [overzicht van de monsters Service Bus]: ../service-bus-messaging/service-bus-samples.md