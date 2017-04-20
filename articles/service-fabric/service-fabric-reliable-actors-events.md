<properties
   pageTitle="Betrouwbare actoren-gebeurtenissen | Microsoft Azure"
   description="Inleiding tot de gebeurtenissen voor betrouwbare Service Fabric-actoren."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-events"></a>Actor-gebeurtenissen
Actor-gebeurtenissen kunnen u best effort om meldingen te verzenden van de actor aan de clients. Actor-gebeurtenissen zijn ontworpen voor communicatie actor-naar-client en mag niet worden gebruikt voor de communicatie van de actor aan acteur.

De volgende codefragmenten wordt aangegeven hoe gebeurtenissen actor in uw toepassing.

Definieert een interface die wordt gepubliceerd door de acteur gebeurtenissen beschreven. Deze interface moet worden afgeleid van de `IActorEvents` interface. De argumenten van de methoden moeten [gegevens contract serialiseerbaar](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). De methoden void retourneren, als de gebeurtenis meldingen één manier en beste inspanningen.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

De gebeurtenissen die zijn gepubliceerd door de actor in de interface van de acteur declareren.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Op de client, voert de gebeurtenis-handler.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

Maken van een proxy naar de acteur die de gebeurtenis publiceert en zich abonneren op gebeurtenissen op de client.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

In geval van failover, de acteur niet via een ander proces of een knooppunt. De acteur-proxy beheert actieve abonnementen en automatisch opnieuw is geabonneerd ze. U kunt het abonnementinterval voor opnieuw via bepalen de `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. Als u wilt opzeggen, gebruik de `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Op de actor, gewoon publiceren de gebeurtenissen zoals ze gebeuren. Als er abonnees van de gebeurtenis, verzendt de runtime actoren het bericht.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Volgende stappen
 - [Acteur herintreding](service-fabric-reliable-actors-reentrancy.md)
 - [Acteur en de prestaties controleren](service-fabric-reliable-actors-diagnostics.md)
 - [Acteur API-documentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Voorbeeld van code](https://github.com/Azure/servicefabric-samples)
