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
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Aan de slag met betrouwbare actoren

> [AZURE.SELECTOR]
- [C# Windows](service-fabric-reliable-actors-get-started.md)
- [Java op Linux](service-fabric-reliable-actors-get-started-java.md)

In dit artikel wordt uitgelegd van Azure Service Fabric betrouwbare actoren en leidt u door het maken en implementeren van een eenvoudige toepassing betrouwbare acteur in Java.

## <a name="installation-and-setup"></a>Installatie en configuratie
Voordat u begint, moet u de Service Fabric-ontwikkelomgeving instellen op uw computer.
Als u instellen wilt, gaat u [aan de slag op een Mac](service-fabric-get-started-mac.md) of [op Linux aan de slag](service-fabric-get-started-linux.md).

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

## <a name="create-an-actor-service"></a>Maak een actor-service
Start een nieuwe Service Fabric-toepassing maken. De Service Fabric-SDK voor Linux bevat een Yeoman generator op de steiger voor de toepassing van een Service weefsel met een stateless service bieden. Start met de Yeoman van de volgende opdracht:

```bash
$ yo azuresfjava
```

Volg de instructies voor het maken van een **Betrouwbare acteur Service**. Voor deze zelfstudie, de naam "HelloWorldActorApplication" van de toepassing en de actor 'HelloWorldActor'. De volgende steiger wordt gemaakt:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Betrouwbare actoren bouwstenen

De basisbegrippen die eerder zijn beschreven vertalen in de bouwstenen van een Actor betrouwbare service.

### <a name="actor-interface"></a>Actor-interface

Dit document bevat de interfacedefinitie van de voor de actor. Deze interface definieert de overeenkomst actor die wordt gedeeld door de uitvoering van de acteur en de clients het aanroepen van de acteur, zodat het meestal verstandig te definiëren op een plaats die gescheiden is van de uitvoering van de acteur en kan worden gedeeld door meerdere andere services of toepassingen.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Actor-service 
Dit document bevat de actor uitvoering en registratiecode actor. Actor-klasse implementeert de actor-interface. Dit is waar de acteur zijn werk doet.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Actor-registratie

De acteur-service moet worden geregistreerd met een servicetype in de runtime Fabric-Service. Om de Actor-Service uit te voeren van de exemplaren voor de actor, moet uw actortype ook met de acteur-Service zijn geregistreerd. De `ActorRuntime` registratiemethode van dit werk uitvoert voor actoren.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {
    
    public static void main(String[] args) throws Exception {
        
        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);
            
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Testclient

Dit is een eenvoudige test clienttoepassing afzonderlijk van de toepassing Fabric-Service voor het testen van uw service actor kan worden uitgevoerd. Dit is een voorbeeld van waar de ActorProxy kan worden gebruikt om te activeren en te communiceren met de acteur exemplaren. Het is niet met uw service krijgen geïmplementeerd.

### <a name="the-application"></a>De toepassing 

Ten slotte verpakt de toepassing de actor-service en andere services die u, in de toekomst samen voor de implementatie toevoegen kunt. De houders van het *ApplicationManifest.xml* en plaats voor het pakket actor service bevat.

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

De Yeoman steigers bevat een script gradle om de toepassing bouwen en bash scripts te implementeren en un-implementatie van de toepassing. Voor het uitvoeren van de toepassing, bouwt u eerst de toepassing met de gradle:

```bash
$ gradle
```

Dit zal leiden tot een toepassingspakket Fabric-Service die kan worden gedistribueerd via Service Fabric Azure CLI. Het script install.sh bevat Azure CLI opdrachten voor het implementeren van het toepassingspakket. Gewoon Voer het script install.sh te implementeren:

```bask
$ ./install.sh
```
