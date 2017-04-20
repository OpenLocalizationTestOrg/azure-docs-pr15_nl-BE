<properties
   pageTitle="Aan de slag met betrouwbare diensten | Microsoft Azure"
   description="Inleiding tot het maken van een toepassing Microsoft Azure Service Fabric met stateless en stateful services."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="vturecek"/>

# <a name="get-started-with-reliable-services"></a>Aan de slag met betrouwbare diensten

> [AZURE.SELECTOR]
- [C# Windows](service-fabric-reliable-services-quick-start.md)
- [Java op Linux](service-fabric-reliable-services-quick-start-java.md)

In dit artikel wordt uitgelegd van betrouwbare configuratieservices Azure Service en leidt u door het maken en implementeren van een eenvoudige betrouwbare Service toepassing geschreven in Java.

## <a name="installation-and-setup"></a>Installatie en configuratie
Voordat u begint, moet u de Service Fabric-ontwikkelomgeving instellen op uw computer.
Als u instellen wilt, gaat u [aan de slag op een Mac](service-fabric-get-started-mac.md) of [op Linux aan de slag](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Basisbegrippen
Om te beginnen met betrouwbare Services, moet u alleen enkele fundamentele concepten te begrijpen:

 - **Servicetype**: dit is de uitvoering van de service. Zij wordt gedefinieerd door de klasse u schrijven die `StatelessService` en een andere code of afhankelijkheden die, samen met een naam en een versienummer.

 - **Benoemd exemplaar**: voor het uitvoeren van uw service, u maakt benoemde exemplaren van het servicetype veel zoals u exemplaren van het type van een klasse maakt. Exemplaren van de service zijn in feite objectinstanties van de klasse die u schrijft. 

 - **Host service**: de service benoemde exemplaren u wilt uitvoeren in een host. De ServiceHost is gewoon een proces waarbij exemplaren van de service kunnen worden uitgevoerd.

 - **Serviceregistratie**: registratie brengt alles samen. Het servicetype moet geregistreerd zijn bij de Service Fabric runtime in een ServiceHost, waardoor weefsel exemplaren worden gemaakt van deze Service uit te voeren.  

## <a name="create-a-stateless-service"></a>Maak een stateless service

Start een nieuwe Service Fabric-toepassing maken. De Service Fabric-SDK voor Linux bevat een Yeoman generator op de steiger voor de toepassing van een Service weefsel met een stateless service bieden. Start met de Yeoman van de volgende opdracht:

```bash
$ yo azuresfjava
```

Volg de instructies voor het maken van een **Betrouwbare Stateless Service**. Voor deze zelfstudie, de naam "HelloWorldApplication" van de toepassing en de service "Hallo wereld". Het resultaat bevat mappen voor de `HelloWorldApplication` en `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>De service implementeren

Open **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Deze klasse definieert het servicetype en een willekeurige code kan worden uitgevoerd. De service-API biedt twee ingangspunten voor uw code:

 - Een open punt methode, genaamd `runAsync()`, waar u kunt beginnen de werkbelasting, met inbegrip van langdurige compute werkbelasting wordt uitgevoerd.

```java
@Override
protected CompletableFuture<?> runAsync() {
    ...
}
```

 - Een mededeling van binnenkomst waar u in uw stapel communicatie van keuze kunt aansluiten. Dit is waar u kunt beginnen aanvragen ontvangen van gebruikers en andere services.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

In deze zelfstudie, gaan we in op de `runAsync()` punt methode. Dit is waar u kunt onmiddellijk beginnen uw code wordt uitgevoerd.

### <a name="runasync"></a>RunAsync

Het platform, roept deze methode wanneer u een exemplaar van een service is geplaatst en kan worden uitgevoerd. De cyclus openen/sluiten van een service-exemplaar kan optreden vaak in de loop van de service als geheel. Dit kan gebeuren om verschillende redenen, waaronder:

- Het systeem wordt verplaatst van de exemplaren van de service voor taakverdeling resource.
- Fouten die voorkomen in uw code.
- De toepassing of het systeem is bijgewerkt.
- De onderliggende hardware optreedt een stroomstoring.

Deze situatie wordt beheerd door Fabric-Service om uw service goed uitgebalanceerde en uiterst beschikbaar.

#### <a name="cancellation"></a>Annulering

Is het van cruciaal belang dat de code in `runAsync()` worden uitgevoerd als Service Fabric kunt stoppen. De `CompletableFuture` geretourneerd door `runAsync()` wordt geannuleerd wanneer Fabric-Service moet de service niet meer kan worden uitgevoerd. In het volgende voorbeeld wordt getoond hoe een gebeurtenis annuleren verwerken: 

```java
    @Override
    protected CompletableFuture<?> runAsync() {

        CompletableFuture<?> completableFuture = new CompletableFuture<>();
        ExecutorService service = Executors.newFixedThreadPool(1);
        
        Future<?> userTask = service.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try
                {
                   logger.log(Level.INFO, this.context().serviceName().toString());
                   Thread.sleep(1000);
                }
                catch (InterruptedException ex)
                {
                    logger.log(Level.INFO, this.context().serviceName().toString() + " interrupted. Exiting");
                    return;
                }
            }
         });
 
        completableFuture.handle((r, ex) -> {
            if (ex instanceof CancellationException) {
                userTask.cancel(true);
                service.shutdown();
            }
            return null;
        });
 
        return completableFuture;
   }
``` 

### <a name="service-registration"></a>Serviceregistratie

Servicetypen moeten geregistreerd zijn bij de Service Fabric-runtime. Het servicetype is gedefinieerd in de `ServiceManifest.xml` en de klasse die u implementeert `StatelessService`. Registratie van de service wordt uitgevoerd in de belangrijkste ingangspunt van het proces. In dit voorbeeld wordt het proces Hoofdgegeven is `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    } 
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration: {0}", ex.toString());
        throw ex;
    }
}
```

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

De Yeoman steigers bevat een script gradle om de toepassing bouwen en bash scripts te implementeren en un-implementatie van de toepassing. Voor het uitvoeren van de toepassing, bouwt u eerst de toepassing met de gradle:

```bash
$ gradle
```

Dit zal leiden tot een toepassingspakket Fabric-Service die kan worden gedistribueerd via Service Fabric Azure CLI. Het script install.sh bevat Azure CLI opdrachten voor het implementeren van het toepassingspakket. Gewoon Voer het script install.sh te implementeren:

```bask
$ ./install.sh
```
