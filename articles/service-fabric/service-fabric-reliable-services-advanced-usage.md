<properties
   pageTitle="Geavanceerd gebruik van betrouwbare diensten | Microsoft Azure"
   description="Meer informatie over Geavanceerd gebruik van de Service van betrouwbare configuratieservices voor extra flexibiliteit in uw diensten."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Geavanceerd gebruik van de betrouwbare programmeermodel voor Services
Azure Service Fabric vereenvoudigt het schrijven en betrouwbare stateless en stateful services beheren. Deze handleiding zal praten over Geavanceerd gebruik van betrouwbare diensten meer controle en flexibiliteit krijgen via de services. Voordat u deze handleiding leest, Lees [De betrouwbare programmeermodel voor Services](service-fabric-reliable-services-introduction.md).

Stateful-en stateless hebben twee primaire ingangspunten voor de gebruikerscode:

 - `RunAsync`is een algemene entrypoint voor uw servicecode.
 - `CreateServiceReplicaListeners`en `CreateServiceInstanceListeners` is voor het openen van communicatie listeners voor aanvragen van clients.
 
Voor de meeste services zijn deze twee ingangspunten voldoende. In zeldzame gevallen wanneer er meer controle over de levenscyclus van een service nodig is, extra lifecycle gebeurtenissen zijn beschikbaar.

## <a name="stateless-service-instance-lifecycle"></a>Levenscyclus van stateless service-exemplaar

Een stateless service lifecycle is erg eenvoudig. Een stateless service kan alleen worden geopend, gesloten of afgebroken. `RunAsync`in een stateless-service wordt uitgevoerd als een service-exemplaar wordt geopend en wanneer een service-exemplaar is gesloten of afgebroken is geannuleerd. 

Hoewel `RunAsync` toereikend te zijn in bijna alle gevallen het openen, sluiten en gebeurtenissen in een stateless service afbreken zijn ook beschikbaar:

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
  OnOpenAsync wordt aangeroepen wanneer het exemplaar van de staatloze wordt gebruikt. Uitgebreide service initialisatietaken kunnen op dit moment worden gestart.

- `Task OnCloseAsync(CancellationToken)`
  OnCloseAsync wordt aangeroepen wanneer het exemplaar van de staatloze gaat zonder problemen worden afgesloten. Dit kan gebeuren wanneer de code van de service wordt bijgewerkt, het exemplaar van de door Netwerktaakverdeling wordt verplaatst of een tijdelijke storing wordt gedetecteerd. OnCloseAsync kan worden gebruikt voor het veilig alle bronnen sluiten, stoppen de achtergrondverwerking, hebt opgeslagen externe staat of bestaande verbindingen sluiten.

- `void OnAbort()`
  OnAbort wordt aangeroepen wanneer het exemplaar van de staatloze geforceerd wordt afgesloten. Dit heet in het algemeen als een permanente storing op het knooppunt wordt aangetroffen of als Fabric-Service niet op betrouwbare wijze beheren van de instantie van de service lifecycle vanwege interne fouten.

## <a name="stateful-service-replica-lifecycle"></a>Stateful service replica lifecycle

De levenscyclus van de replica van een stateful service is veel complexer dan een stateless service-exemplaar. Bovendien breken gebeurtenissen openen en sluiten, ondergaat een replica stateful service verandert tijdens de levensduur. Als een replica stateful service rol, verandert de `OnChangeRoleAsync` gebeurtenis wordt geactiveerd:

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
  OnChangeRoleAsync wordt opgeroepen als de replica stateful-service rol, bijvoorbeeld primaire of secundaire wijzigen. Primaire replica's schrijven status krijgen (mogen maken en schrijven naar betrouwbare collecties). Secundaire replica's krijgen leesstatus (kan alleen worden gelezen uit de bestaande betrouwbare collecties). De meeste wordt in een stateful-service uitgevoerd op de primaire replica. Secundaire replica's kunnen uitvoeren validatie alleen-lezen, het genereren van rapporten, datamining of andere taken alleen-lezen.

In de stateful-service alleen de primaire replica schrijftoegang heeft tot de staat en is dus in het algemeen als werkelijke hoeveelheid werk door de service wordt uitgevoerd. De `RunAsync` methode in een stateful-service wordt uitgevoerd alleen als de service stateful replica primair is. De `RunAsync` methode wordt geannuleerd wanneer de rol van een primaire replica gewijzigd van primaire, alsmede tijdens de gebeurtenissen sluiten en afbreken. 

Met behulp van de `OnChangeRoleAsync` gebeurtenis kunt u werk afhankelijk van replica rol ook in reactie op de rolwijziging uitgevoerd.

Een stateful-service biedt ook dezelfde vier lifecycle gebeurtenissen als stateless service met dezelfde semantiek en use-cases:

- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`



## <a name="next-steps"></a>Volgende stappen
Voor meer geavanceerde onderwerpen die betrekking hebben op Service Fabric, Zie de volgende artikelen:

- [Stateful betrouwbare Services configureren](service-fabric-reliable-services-configuration.md)

- [Service Fabric gezondheid-Inleiding](service-fabric-health-introduction.md)

- [Statusrapporten systeem gebruiken voor het oplossen van problemen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [Configureren van Services met Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-configure-services.md)
