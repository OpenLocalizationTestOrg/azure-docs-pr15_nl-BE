<properties
   pageTitle="Testbaarheid: Service communicatie | Microsoft Azure"
   description="Services communicatie is een essentiële integratie van een Service Fabric-toepassing. In dit artikel wordt beschreven, overwegingen bij het ontwerpen en testen technieken."
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
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-testability-scenarios-service-communication"></a>Service Fabric testbaarheid scenario's: communicatie-Service

Microservices en servicegeoriënteerde architectuur stijlen vlak natuurlijk in Azure Service Fabric. In deze gedistribueerde architectuur microservice samengestelde toepassingen meestal bestaan uit meerdere services die nodig zijn om met elkaar te communiceren. In zelfs de meest eenvoudige gevallen hebt u in het algemeen ten minste een stateless webservice en een stateful data storage-service die moeten communiceren.

Services communicatie is een essentiële integratie van een toepassing, omdat elke service een externe API met andere services bevat. Werken met een reeks API grenzen waarbij I/O in het algemeen moet u zorgvuldig te werk gaan, met een goede hoeveelheid testen en valideren.

Er zijn talrijke wanneer de servicegrenzen van deze samen in een gedistribueerd systeem bedrading:

 - *Transportprotocol*. U gebruikt HTTP voor verhoogde interoperabiliteit of een aangepaste binaire protocol voor een maximale doorvoer?
 - *Fouten afhandelen*. Hoe permanente en tijdelijke fouten worden verwerkt? Wat er gebeurt wanneer een service wordt verplaatst naar een ander knooppunt?
 - *Time-outs en latentie*. In toepassingen combineren, hoe elke laag service omgaan latentie via de stack en voor de gebruiker?

Of u een van de ingebouwde service communicatie-onderdelen die door de Service Fabric of bouwen van uw eigen, testen van de interacties tussen de diensten is van cruciaal belang om ervoor te zorgen dat de tolerantie in uw toepassing.

## <a name="prepare-for-services-to-move"></a>Voorbereiden voor services verplaatsen

Exemplaren van de service kunnen na verloop van tijd navigeren. Dit geldt vooral wanneer ze zijn geconfigureerd met laden metrics voor balancing optimale resource aangepaste afgestemd. Fabric-service wordt uw service-exemplaren te maximaliseren hun beschikbaarheid zelfs tijdens upgrades, failover, schalen en andere situaties die zich in de loop van een gedistribueerd systeem voordoen.

Als services in het cluster verplaatsen, moeten uw clients en andere services worden voorbereid om twee scenario's wanneer ze contact met een service opnemen:

- De service-exemplaar of de partitie de replica is verplaatst sinds de laatste keer dat u deze hebt gehad. Dit is een normaal onderdeel van een service lifecycle en dit gebeurt tijdens de levensduur van uw toepassing worden verwacht.
- De service-exemplaar of de partitie-replica is bezig met het verplaatsen. Hoewel failover van een service uit één knooppunt in Service weefsel zeer snel optreedt, kan er een vertraging in de beschikbaarheid als onderdeel van de communicatie van uw service start traag op.

Verwerking van deze scenario's zonder problemen is belangrijk voor een soepel functionerend systeem. Als u dit doet, houd rekening met:

- Elke service die kan worden aangesloten op heeft een *adres* waarop deze luistert (bijvoorbeeld HTTP of WebSockets). Als u een service-exemplaar of een partitie verplaatst, wordt het eindpunt van het adres verandert. (Het wordt verplaatst naar een ander knooppunt met een ander IP-adres.) Als u de ingebouwde communicatie-onderdelen gebruikt, verwerkt ze opnieuw omzetten service adressen voor u.
- Kan er een tijdelijke toename van latentie service als het starten van de service-exemplaar van de listener opnieuw. Dit is afhankelijk van hoe snel de listener door de service wordt geopend nadat het service-exemplaar wordt verplaatst.
- Worden bestaande verbindingen moeten worden gesloten en opnieuw geopend nadat de service is geopend op een nieuw knooppunt. Een correcte knooppunt afsluiten of opnieuw opstarten kunt tijd voor bestaande verbindingen worden ' elegant ' afsluiten.

### <a name="test-it-move-service-instances"></a>Testen: service-exemplaren verplaatsen

Met de Service-Fabric testbaarheid's kunt ontwerpen een Testscenario om te testen in deze situaties op verschillende manieren:

1. Verplaats de primaire replica van een stateful service.

    De primaire replica van de partitie van een stateful-service kan om een aantal redenen worden verplaatst. Gebruik deze optie om de primaire replica van een specifieke partitie om te zien hoe uw services om de verplaatsing te reageren op een zeer gecontroleerde manier.

    ```powershell

    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService

    ```

2. Een knooppunt stopt.

    Wanneer een knooppunt wordt gestopt, wordt alle exemplaren van de service of partities die op dat knooppunt naar een van de beschikbare knooppunten in het cluster zijn Fabric-Service verplaatst. Gebruik deze optie voor het testen van een situatie waarbij een knooppunt van het cluster en alle exemplaren van de service de verloren en replica's op dat knooppunt moeten verplaatsen.

    U kunt een knooppunt stopt met behulp van de cmdlet PowerShell **Stop ServiceFabricNode** :

    ```powershell

    PS > Restart-ServiceFabricNode -NodeName Node_1

    ```

## <a name="maintain-service-availability"></a>Beschikbaarheid van Services onderhouden

Fabric-Service is ontworpen als een platform, beschikbaarheid van de diensten. Maar in extreme gevallen onderliggende infrastructuur nog steeds de oorzaak niet beschikbaar. Het is belangrijk deze scenario's te testen.

Een quorum-systeem stateful services gebruiken om te repliceren, staat voor een hoge beschikbaarheid. Dit betekent dat een quorum van replica's beschikbaar zijn moet voor het uitvoeren van schrijfbewerkingen. In zeldzame gevallen, zoals een wijdverbreide hardwarefout een quorum van replica's mogelijk niet beschikbaar. In deze gevallen worden niet schrijven-bewerkingen uitvoeren, maar is nog steeds lezen bewerkingen uitvoeren.

### <a name="test-it-write-operation-unavailability"></a>Testen: schrijf niet beschikbaar voor bewerking

Met de hulpprogramma's voor testbaarheid in Fabric-Service kunt u een fout die het quorum verloren gaan als een test induceert invoeren. Hoewel deze situatie niet vaak voorkomt, is het belangrijk dat clients en services die afhankelijk van een stateful service zijn worden opgesteld voor situaties die niet kunnen maken, indien zij verzoeken te schrijven. Het is ook belangrijk dat de stateful-service zelf zich bewust van deze mogelijkheid is en probleemloos als mensen opbellen communiceren kan.

U kunt quorum verlies veroorzaken via de cmdlet PowerShell **Invoke-ServiceFabricPartitionQuorumLoss** :

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

In dit voorbeeld stelt `QuorumLossMode` op `QuorumReplicas` om aan te geven dat we quorum verlies veroorzaken willen zonder het nemen van alle replica's. Op deze manier leesbewerkingen zijn nog steeds mogelijk. Als u wilt testen in een scenario waarbij een volledige partitie niet beschikbaar is, kunt u deze schakeloptie instellen op `AllReplicas`.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over acties testbaarheid](service-fabric-testability-actions.md)

[Meer informatie over scenario's voor testbaarheid](service-fabric-testability-scenarios.md)
