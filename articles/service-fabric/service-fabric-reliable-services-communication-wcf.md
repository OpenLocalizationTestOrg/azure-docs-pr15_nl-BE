<properties
   pageTitle="Betrouwbare WCF Services communicatie stack | Microsoft Azure"
   description="De ingebouwde WCF communicatie stack in Fabric-Service biedt client-service WCF communicatie voor betrouwbare diensten."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="bharatn"/>

# <a name="wcf-based-communication-stack-for-reliable-services"></a>WCF-communicatie-stack voor betrouwbare diensten
Het kader van betrouwbare diensten kan auteurs service kiest u de stapel communicatie die u wilt gebruiken voor de service. Ze kunnen in de stack communicatie van hun keuze via de **ICommunicationListener** die wordt geretourneerd door de methode [CreateServiceReplicaListeners of CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) aansluiten. Het framework is een implementatie van de communicatie-stack gebaseerd op Windows Communication Foundation (WCF) voor service-auteurs die willen gebruiken WCF-communicatie.

## <a name="wcf-communication-listener"></a>WCF communicatie Listener
De WCF-specifieke uitvoering van **ICommunicationListener** wordt verstrekt door de klasse **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** .

Lest zegt we een servicecontract van het type hebben`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Een listener WCF communicatie maken we in de service op de volgende wijze.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>Schrijven van clients voor de stack WCF-communicatie
Het kader biedt voor het schrijven van clients communiceren met services met behulp van WCF, **WcfClientCommunicationFactory**, de WCF-specifieke implementatie van [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

Het communicatiekanaal WCF toegankelijk is vanuit de **WcfCommunicationClient** gemaakt door de **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Clientcode kunt gebruiken de **WcfCommunicationClientFactory** en de **WcfCommunicationClient** die **ServicePartitionClient** om te bepalen van het service-eindpunt en communiceren met de service wordt geïmplementeerd.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
>[AZURE.NOTE] De standaard ServicePartitionResolver, wordt ervan uitgegaan dat de client in hetzelfde cluster als de service wordt uitgevoerd. Als dat niet het geval is, een ServicePartitionResolver-object maken en in de cluster verbinding eindpunten.

## <a name="next-steps"></a>Volgende stappen
* [Externe procedureaanroep met betrouwbare diensten remoting](service-fabric-reliable-services-communication-remoting.md)

* [Web API met OWIN in betrouwbare Services](service-fabric-reliable-services-communication-webapi.md)

* [Beveiliging van communicatie voor betrouwbare diensten](service-fabric-reliable-services-secure-communication.md)
