<properties
   pageTitle="Polymorfisme in het kader van betrouwbare actoren | Microsoft Azure"
   description="Hiërarchieën van .NET interfaces en typen in het kader van betrouwbare actoren om functionaliteit en API definities opnieuw te bouwen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/07/2016"
   ms.author="seanmck"/>

# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polymorfisme in het kader van betrouwbare actoren

De betrouwbare actoren framework kunt u bouwen met grotendeels dezelfde technieken die u in object-georiënteerd ontwerp gebruikt actoren. Een van deze technieken is het polymorfisme kunt typen en-interfaces voor het overnemen van meer algemene ouders. Overname in het kader van betrouwbare actoren volgt meestal de .NET-model met een paar extra beperkingen.

## <a name="interfaces"></a>Interfaces

Het kader van betrouwbare actoren moet u ten minste één interface moet worden geïmplementeerd op uw actortype definiëren. Deze interface wordt gebruikt voor het genereren van een proxyklasse die door clients kan worden gebruikt om te communiceren met de hoofdpersonen. Interfaces kunnen overnemen van andere interfaces als elke interface die wordt geïmplementeerd door een actortype en alle van de bovenliggende items worden uiteindelijk afgeleid van IActor. IActor is het platform gedefinieerde basisinterface voor actoren. Dus het klassieke polymorfisme voorbeeld met behulp van shapes kan als volgt uitzien:

![Hiërarchie voor de actoren van de shape Interface][shapes-interface-hierarchy]


## <a name="types"></a>Typen

Ook kunt u een hiërarchie van acteur typen die zijn afgeleid van de klasse base Actor die wordt geleverd door het platform. In het geval van vormen, moet u mogelijk een base `Shape` type:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

Subtypes van `Shape` methoden van de basis kunt negeren.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Opmerking de `ActorService` kenmerk van het actortype. Dit kenmerk instrueert de betrouwbare acteur framework dat een service voor het hosten van actoren van dit type automatisch moeten worden gemaakt. In sommige gevallen wilt u mogelijk een basistype dat uitsluitend is bedoeld voor het delen van functionaliteit met subtypen en nooit wordt gebruikt voor het starten van concrete actoren maken. In dat geval moet u de `abstract` trefwoord om aan te geven dat u nooit een actor op basis van dat type maakt.


## <a name="next-steps"></a>Volgende stappen

- Zie [hoe de Service Fabric-platform maakt gebruik van het betrouwbare actoren kader](service-fabric-reliable-actors-platform.md) om de betrouwbaarheid, schaalbaarheid en consistente status.
- Meer informatie over de [levenscyclus van de actor](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
