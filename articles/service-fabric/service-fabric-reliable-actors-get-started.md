<properties
   pageTitle="Aan de slag met betrouwbare Service Fabric-actoren | Microsoft Azure"
   description="Deze zelfstudie doorloopt u de stappen voor het maken, debuggen en implementeren van een eenvoudige actor-service met behulp van betrouwbare Service Fabric-actoren."
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
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Aan de slag met betrouwbare actoren

> [AZURE.SELECTOR]
- [C# Windows](service-fabric-reliable-actors-get-started.md)
- [Java op Linux](service-fabric-reliable-actors-get-started-java.md)

In dit artikel wordt uitgelegd van Azure Service Fabric betrouwbare actoren en leidt u door het maken, debuggen en distribueren van een eenvoudige toepassing betrouwbare acteur in Visual Studio.

## <a name="installation-and-setup"></a>Installatie en configuratie
Voordat u begint, zorg ervoor dat de Service Fabric-ontwikkelomgeving instellen op uw computer.
Als u wilt instellen, Zie gedetailleerde instructies over [het instellen van de ontwikkelomgeving](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Basisbegrippen
Om te beginnen met betrouwbare actoren moet u alleen een paar eenvoudige concepten te begrijpen:

 * **Actor-service**. Betrouwbare actoren zijn verpakt in een betrouwbare diensten die kunnen worden geïmplementeerd in de Service Fabric-infrastructuur. Acteur exemplaren worden in een exemplaar van benoemde service geactiveerd.
 
 * **Registratie van de actor**. Met betrouwbare Services moet een betrouwbare Actor-service tijdens de runtime Fabric-Service worden geregistreerd. Bovendien moet het actortype worden geregistreerd bij de uitvoering van de Actor.
 
 * **Actor-interface**. De acteur-interface wordt gebruikt voor het definiëren van een sterk getypeerde openbare interface van een acteur. In de terminologie van het model betrouwbare acteur definieert de actor interface de typen berichten die de actor kan begrijpen en verwerken. De acteur-interface wordt gebruikt door andere actoren en clienttoepassingen "" (asynchroon) om berichten te verzenden naar de actor. Betrouwbare actoren kunnen meerdere interfaces implementeren.
 
 * **Klasse ActorProxy**. De klasse ActorProxy wordt gebruikt door clienttoepassingen aan te roepen de methoden die toegankelijk zijn via de interface van de actor. De klasse ActorProxy bevat twee belangrijke functies:
    * Naamomzetting: kan de actor vinden in het cluster (zoeken naar het knooppunt van het cluster waar deze wordt gehost).
    * Fout bij verwerking: kan opnieuw aanroepen van de methode en de actor locatie te zetten na bijvoorbeeld een fout waarvoor de actor moet worden verplaatst naar een ander knooppunt in het cluster.

De volgende regels die betrekking op de actor interfaces hebben worden opgemerkt:

- Interfacemethoden actor kunnen niet worden overbelast.
- Actor-interface die methoden niet van hebben moeten ref of optionele parameters.
- Algemene interfaces worden niet ondersteund.

## <a name="create-a-new-project-in-visual-studio"></a>Maak een nieuw project in Visual Studio
Nadat u de Service Fabric-hulpprogramma's voor Visual Studio hebt geïnstalleerd, kunt u nieuwe projecttypen. Het nieuwe project zijn onder de categorie **wolk** van het dialoogvenster **Nieuw Project** .


![Tools voor Visual Studio - project nieuwe service Fabric][1]

In het volgende dialoogvenster kunt u het type project dat u wilt maken.

![Service Fabric project-sjablonen][5]

We gebruiken de betrouwbare Service Fabric-actoren-service voor het project HelloWorld.

Nadat u de oplossing hebt gemaakt, ziet u de volgende structuur:

![Service-structuur projectstructuur][2]

## <a name="reliable-actors-basic-building-blocks"></a>Betrouwbare actoren bouwstenen

Een typische actoren betrouwbare oplossing bestaat uit drie projecten:

* **De application-project (MyActorApplication)**. Dit is het project dat pakketten van alle services samen voor de implementatie. De *ApplicationManifest.xml* en PowerShell-scripts voor het beheer van de toepassing bevat.

* **Het project interface (MyActor.Interfaces)**. Dit is het project met de interfacedefinitie van de voor de actor. In het MyActor.Interfaces-project kunt u de interfaces die door de actoren in de oplossing wordt gebruikt. De interfaces van de actor kunnen worden gedefinieerd in elk project met de naam, maar de interface bepaalt het contract actor die wordt gedeeld door de uitvoering van de acteur en de clients het aanroepen van de acteur, dus het meestal verstandig te definiëren in een assembly die los staat van de uitvoering van de acteur en kan worden gedeeld door meerdere andere projecten.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **De acteur service project (MyActor)**. Dit is het project gebruikt voor het definiëren van de Service Fabric-service die als host voor de actor is. Het bevat de uitvoering van de actor. De implementatie van een actor een klasse die is afgeleid van het basistype is `Actor` en implementeert de interface (s) die zijn gedefinieerd in het MyActor.Interfaces-project. Een actor-klasse hanteert een constructor die accepteert ook een `ActorService` exemplaar en een `ActorId` en geeft deze door aan de base `Actor` klasse. Hiermee constructor afhankelijkheid injectie van platform afhankelijkheden.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

De acteur-service moet worden geregistreerd met een servicetype in de runtime Fabric-Service. Om de Actor-Service uit te voeren van de exemplaren voor de actor, moet uw actortype ook met de acteur-Service zijn geregistreerd. De `ActorRuntime` registratiemethode van dit werk uitvoert voor actoren.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Als u van een nieuw project in Visual Studio Start en u slechts één definitie van de acteur hebt, is de registratie standaard opgenomen in de code die Visual Studio is gegenereerd. Als u andere actoren in de service definieert, moet u de registratie van de acteur toevoegen met behulp van:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [AZURE.TIP] De Service Fabric actoren runtime zendt bepaalde [gebeurtenissen en prestatiemeteritems met betrekking tot de methoden van de actor](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Ze zijn nuttig bij diagnostiek en controle van de prestaties.


## <a name="debugging"></a>Foutopsporing

De Service Fabric-hulpprogramma's voor Visual Studio ondersteuning voor foutopsporing op de lokale computer. U kunt een sessie voor foutopsporing starten kunt u door op de toets F5. Visual Studio maakt (indien nodig) pakketten. Ook de toepassing op de lokale Service Fabric '-cluster implementeert en de debugger.

Tijdens de implementatie ziet u de voortgang in het venster **uitvoer** .

![Foutopsporing uitvoervenster Fabric-service][3]


## <a name="next-steps"></a>Volgende stappen
 - [Hoe betrouwbaar actoren het platform Fabric-Service gebruiken](service-fabric-reliable-actors-platform.md)
 - [Statusbeheer actor](service-fabric-reliable-actors-state-management.md)
 - [Acteur lifecycle en garbage collection](service-fabric-reliable-actors-lifecycle.md)
 - [Acteur API-documentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Voorbeeld van code](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
