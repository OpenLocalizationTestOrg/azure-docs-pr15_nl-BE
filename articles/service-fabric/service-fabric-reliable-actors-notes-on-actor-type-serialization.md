<properties
   pageTitle="Betrouwbare actoren toelichting actor Typ serialisatie | Microsoft Azure"
   description="Bespreekt de basisvereisten voor serializable klassen die kunnen worden gebruikt voor het definiëren van interfaces en betrouwbare Service Fabric-actoren-lidstaten definiëren"
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
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Toelichting op de betrouwbare Service Fabric-actoren Typ serialisatie


De argumenten van alle methoden, resultaattypen van de taken die door elke methode in een actor-interface en de objecten die zijn opgeslagen in beheer van de status van een actor moeten [Gegevenscontract serializable](https://msdn.microsoft.com/library/ms731923.aspx)... Dit geldt ook voor de argumenten van de methoden die zijn gedefinieerd in de [actor gebeurtenisinterfaces](service-fabric-reliable-actors-events.md#actor-events). (Actor gebeurtenis interfacemethoden altijd void retourneren.)

## <a name="custom-data-types"></a>Aangepaste gegevenstypen

In dit voorbeeld wordt de volgende actor interface definieert een methode geeft als resultaat een aangepast gegevenstype genoemd `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

De interface is impelemented door een actor die gebruikmaakt van de Manager staat voor het opslaan van een `VoicemailBox` object:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

In dit voorbeeld wordt de `VoicemailBox` object serienummer wanneer:
 - Het object wordt verzonden tussen een actor en een beller.
 - Het object wordt opgeslagen in de Manager van de staat waar het op schijf vastgelegd en gerepliceerd naar andere knooppunten.
 
Het betrouwbare acteur framework gebruikt DataContract serialisatie. Daarom moeten de aangepaste objecten en hun leden aangevuld worden met de **DataContract** en **DataMember** attributen, respectievelijk

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Volgende stappen
 - [Acteur lifecycle en garbage collection](service-fabric-reliable-actors-lifecycle.md)
 - [Actor-timers en herinneringen](service-fabric-reliable-actors-timers-reminders.md)
 - [Actor-gebeurtenissen](service-fabric-reliable-actors-events.md)
 - [Acteur herintreding](service-fabric-reliable-actors-reentrancy.md)
 - [Acteur polymorfisme en objectgeoriënteerde ontwerppatronen](service-fabric-reliable-actors-polymorphism.md)
 - [Acteur en de prestaties controleren](service-fabric-reliable-actors-diagnostics.md)
