<properties
   pageTitle="Betrouwbare collecties | Microsoft Azure"
   description="Service stateful configuratieservices bieden betrouwbare collecties waarmee u uiterst beschikbaar, schaalbaar en lage latentie cloud-toepassingen schrijven."
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Inleiding tot betrouwbare collecties in Azure Service stateful configuratieservices

Betrouwbare collecties kunnen u uiterst beschikbaar, schaalbaar en lage latentie cloud-toepassingen schrijven, alsof u één computertoepassingen schrijft. De klassen in de naamruimte **Microsoft.ServiceFabric.Data.Collections** bieden een set van out-of-the-box-collecties die automatisch uw staat uiterst beschikbaar. Ontwikkelaars moeten het programma alleen naar de betrouwbare verzameling API's en laat u betrouwbare collecties beheren de gerepliceerde en lokale staat.

Het belangrijkste verschil tussen betrouwbare collecties en andere technologieën voor hoge beschikbaarheid (zoals bestand Vgx. tabel Azure service en Azure Queue-service) is dat de staat lokaal in het exemplaar van de blijft tijdens het ook uiterst beschikbaar gesteld. Dit betekent dat:

- Alle leesbewerkingen zijn lokale, wat resulteert in een lage latentie en hoge gegevensdoorvoer leest.
- Alle schrijfbewerkingen tot het minimum aantal IOs netwerk, wat in een lage latentie resulteert en hoge gegevensdoorvoer schrijft.

![Afbeelding van de ontwikkeling van de collecties.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Betrouwbare verzamelingen kunnen worden beschouwd als de natuurlijke ontwikkeling van de **System.Collections** -klassen: een nieuwe set van collecties die zijn ontworpen voor de cloud en voor meerdere computers toepassingen zonder toename van complexiteit voor ontwikkelaars. Als zodanig zijn betrouwbare collecties:

- Gerepliceerd: Staat wijzigingen zijn gerepliceerd voor hoge beschikbaarheid.
- Behouden: Is de vastgelegde gegevens op schijf voor duurzaamheid tegen grootschalige storingen (bijvoorbeeld datacenter stroomstoringen).
- Asynchrone: API's zijn asynchroon, om ervoor te zorgen dat threads niet worden geblokkeerd bij het aangaan van i/o.
- Transactionele: API's maken gebruik van de onttrekking van transacties zodat u gemakkelijk meerdere betrouwbare collecties binnen een service kunt beheren.

Betrouwbare collecties bevatten sterke samenhang waarborgen uit de doos te vereenvoudigen redenering over de toepassingsstatus.
Sterke samenhang wordt bereikt door ervoor te zorgen dat de transactie doorvoeracties voltooien nadat de gehele transactie is aangemeld een quorum meerderheid van replica's, met inbegrip van de primaire.
Toepassingen kunnen voor zwakkere consistentie, terug naar de client/aanvrager erkent voordat de commit asynchroon wordt geretourneerd.

De betrouwbare collecties API's zijn een evolutie van gelijktijdige collecties API's (gevonden in de naamruimte **System.Collections.Concurrent** ):

- Asynchrone: Retourneert een taak omdat, in tegenstelling tot gelijktijdige collecties, de bewerkingen worden gerepliceerd en bewaard.
- Geen uitvoerparameters: gebruikt `ConditionalValue<T>` naar een Boole-waarde en een waarde in plaats van uitvoerparameters retourneren. `ConditionalValue<T>`lijkt op `Nullable<T>` , maar geen T een struct worden vereist.
- Transacties: Wordt een transactieobject dat de gebruiker kan de groep acties op meerdere betrouwbare in een transactie.

**Microsoft.ServiceFabric.Data.Collections** bevat nu twee collecties:

- [Betrouwbare woordenboek](https://msdn.microsoft.com/library/azure/dn971511.aspx): een gerepliceerde transactionele en asynchrone collectie sleutel/waarde-paren. Net als bij de **ConcurrentDictionary**, zowel de sleutel als de waarde kunnen worden van een willekeurig type.
- [Betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx): Hiermee geeft u een gerepliceerde transactionele en asynchrone strikte first-in, binnenkomst (FIFO) wachtrij. Net als bij **ConcurrentQueue**, de waarde van elk type kan zijn.

## <a name="isolation-levels"></a>Isolatieniveaus
Het isolatieniveau bepaalt de mate waarin de transactie kunnen geïsoleerd van de wijzigingen die zijn aangebracht door andere transacties worden.
Er zijn twee isolatieniveaus die worden ondersteund in betrouwbare collecties:

- **Herhaalbare leesbewerking**: Hiermee geeft u instructies kunnen gegevens die zijn gewijzigd, maar nog niet zijn doorgevoerd in andere transacties worden gelezen en dat geen andere transacties die door de huidige transactie is gelezen wijzigen kunnen totdat de huidige transactie is voltooid. Zie [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx)voor meer informatie.
- **Momentopname**: geeft aan dat gegevens die worden gelezen door elke instructie in een transactie de transactioneel consistent versie van de gegevens die bij het begin van de transactie bestonden.
De transactie herkent alleen de gegevenswijzigingen die zijn gepleegd vóór het begin van de transactie.
Gegevenswijzigingen in andere transacties na het begin van de huidige transactie zijn niet zichtbaar voor instructies bij de huidige transactie uitvoeren.
Het effect is als de instructies in een transactie een momentopname van de vastgelegde gegevens ophalen, zoals deze bestond aan het begin van de transactie.
Momentopnamen zijn consistent over betrouwbare collecties.
Zie [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx)voor meer informatie.

Betrouwbare collecties kiest automatisch het isolatieniveau aan voor een bepaalde leesbewerking afhankelijk van de bewerking en de rol van de replica op het moment van de transactie is gemaakt.
Hieronder ziet u de tabel ziet u standaardwaarden op siteniveau isolatie voor wachtrij- en betrouwbaar woordenboek.

| Bewerking \ rol      | Primaire          | Secundaire        |
| --------------------- | :--------------- | :--------------- |
| Lees enkele entiteit    | Herhaalbare leesbewerking  | Momentopname         |
| Opsomming \ tellen   | Momentopname         | Momentopname         |

>[AZURE.NOTE] Veelvoorkomende voorbeelden voor één entiteit bewerkingen zijn `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.

Zowel de betrouwbare woordenlijst en de wachtrij voor betrouwbare ondersteuning lees je schrijft.
Met andere woorden, is alle schrijven binnen een transactie zichtbaar voor een volgende lezen die tot dezelfde transactie behoort.

## <a name="locking"></a>Vergrendelen
Betrouwbare collecties, alle transacties zijn twee fasen: een transactie heeft geen vergrendelingen de deze totdat de transactie wordt beëindigd met een afgebroken of een commit heeft verworven.

Betrouwbare woordenlijst gebruikt rijniveau vergrendeling voor alle bewerkingen van de rechtspersoon.
Betrouwbare wachtrij, ruilt gelijktijdigheid voor strikte transactionele FIFO-eigenschap.
Betrouwbare wachtrij wordt gebruikt voor bewerking niveau vergrendelingen waarmee één transactie met `TryPeekAsync` en/of `TryDequeueAsync` en één transactie met `EnqueueAsync` tegelijk.
Houd er rekening mee dat als u wilt behouden, FIFO, als een `TryPeekAsync` of `TryDequeueAsync` ooit neemt dat de betrouwbare wachtrij leeg is, zij zal ook vergrendelen `EnqueueAsync`.

Schrijf altijd worden exclusieve vergrendeling.
Voor leesbewerkingen, de vergrendeling is afhankelijk van een aantal factoren.
Elke leesbewerking gedaan met behulp van Snapshot-isolatie is lock vrij.
Een herhaalbare leesbewerking standaard duurt gedeelde vergrendelingen.
Voor elke leesbewerking die herhaalbare leesbewerking ondersteunt, kan de gebruiker echter verzoeken voor een Update vergrendeling in plaats van de gedeelde vergrendeling.
De vergrendeling van een Update is een asymmetrische vergrendelen om te voorkomen dat een veelvoorkomende vorm van impasse die optreedt wanneer meerdere transacties bronnen voor mogelijke updates op een later tijdstip worden vergrendeld.

Hieronder vindt u de vergrendeling compatibility matrix:

| Aanvraag \ verleend | Geen         | Gedeeld       | Update      | Exclusieve    |
| ----------------- | :----------- | :----------- | :---------- | :----------- |
| Gedeeld            | Geen conflict  | Geen conflict  | Conflict    | Conflict     |
| Update            | Geen conflict  | Geen conflict  | Conflict    | Conflict     |
| Exclusieve         | Geen conflict  | Conflict     | Conflict    | Conflict     |

Houd er rekening mee dat een time-argument voor de betrouwbare collecties API's voor deadlock-detectie wordt gebruikt.
Bijvoorbeeld probeert twee transacties (T1 en T2) te lezen en bijwerken van K1.
Is het mogelijk deze impasse, omdat ze beide uiteindelijk met de gedeelde vergrendeling.
In dit geval wordt een of beide van de bewerkingen-time-out.

Houd er rekening mee dat het bovenstaande scenario in de impasse een goed voorbeeld is van hoe een vergrendeling Update deadlocks kunt voorkomen.

## <a name="persistence-model"></a>Persistentie model
Volg een persistentie model dat wordt genoemd op het logboekbestand en de controlepunten de betrouwbare staat Manager en betrouwbare collecties.
Dit is een model waarbij elke wijziging staat op schijf vastgelegd en alleen in het geheugen wordt toegepast.
De volledige staat zelf wordt slechts af en toe behouden (ook Controlepunt).
Het voordeel is dat delta's worden omgezet in opeenvolgende alleen toevoegen schrijven naar de schijf voor betere prestaties.

Voor een beter begrip van het logbestand en controlepuntgegevens model, we eerst kijken naar het oneindige schijf scenario.
De betrouwbare staat Manager registreert elke bewerking voordat deze worden gerepliceerd.
Hierdoor is de betrouwbare collectie toe te passen, alleen de bewerking in het geheugen.
Aangezien de logboeken worden doorgevoerd wanneer de replica is mislukt en moet opnieuw worden gestart, heeft de Manager van de betrouwbare staat voldoende informatie in de logboeken voor alle bewerkingen in die de replica heeft verloren.
Als de schijf oneindig is, logboekrecords moeten nooit worden verwijderd en de collectie betrouwbare moet beheren, alleen de in het geheugen staat.

Nu laten we het scenario eindige schijf.
Als de records worden verzameld, wordt de betrouwbare staat Manager schijfruimte.
Voordat dat gebeurt, moet de betrouwbare staat Manager afkappen van het logboek ruimte te maken voor de nieuwe records.
Dit vraagt om de betrouwbare collecties aan checkpoint hun status in het geheugen naar schijf.
Het is de betrouwbare collecties verantwoordelijkheid om de toestand die tot dat punt.
Zodra de betrouwbare collecties hun controlepunten voltooien, kan de Manager van de betrouwbare staat het logboek voor het vrijmaken van schijfruimte afkappen.
Op deze manier, wanneer de replica moet opnieuw worden gestart, betrouwbare collecties worden hersteld met de controlepunt staat en de betrouwbare staat Manager zal herstellen en alle wijzigingen die zich hebben voorgedaan sinds het controlepunt af te spelen.

>[AZURE.NOTE] Een andere waarde toevoegen van controlepuntbeheer is dat verbetert de prestaties van herstel in voorkomende gevallen.
Dit komt doordat de controlepunten bevatten alleen de meest recente versies.

## <a name="recommendations"></a>Aanbevelingen

- Een object van het type aangepaste geretourneerd door leesbewerkingen niet wijzigen (bijv. `TryPeekAsync` of `TryGetValueAsync`). Betrouwbare collecties, net als bij gelijktijdige collecties, een verwijzing retourneren naar de objecten en geen kopie.
- Doe diepe kopie het geretourneerde object van een aangepast type voordat u deze wijzigt. Aangezien structs en ingebouwde typen pass op waarde, hoeft niet te doen een diepe kopie van deze.
- Gebruik geen `TimeSpan.MaxValue` voor time-outs. Time-outs moeten worden gebruikt voor het vaststellen van deadlocks.
- Gebruik een transactie niet nadat het is begaan, afgebroken, of verwijderd.
- Gebruik een opsomming niet buiten de scope van de transactie die is gemaakt.
- Maak een transactie binnen een andere transactie geen `using` instructie omdat dit leiden impasses tot kan.
- Kan ervoor zorgen dat uw `IComparable<TKey>` uitvoering correct is. Het systeem neemt afhankelijkheid dit voor het samenvoegen van controlepunten.
- Gebruik Update vergrendelen bij het lezen van een artikel met een voornemen om bij te werken om te voorkomen dat een bepaalde categorie van deadlocks.
- Overweeg het gebruik van back-up en herstel functionaliteit om noodherstel.
- Vermijd vermenging enkele entiteit bewerkingen en entiteit met meerdere bewerkingen (zoals `GetCountAsync`, `CreateEnumerableAsync`) in dezelfde transactie als gevolg van de verschillende isolatieniveaus.

Hier volgen enkele dingen rekening houden:

- De standaard time-outperiode is 4 seconden voor alle betrouwbare verzameling API's. De meeste gebruikers moeten dit niet negeren.
- Het standaardtoken annulering is `CancellationToken.None` in alle betrouwbare collecties met API's.
- De parameter type sleutel (*TKey*) voor een betrouwbare woordenlijst moet correct worden geïmplementeerd `GetHashCode()` en `Equals()`. Sleutels moeten onveranderbaar zijn.
- Voor maximale beschikbaarheid voor de betrouwbare collecties, moet elke service ten minste een doel en een minimale grootte van 3 replicaset.
- Leesbewerkingen op de secundaire server kunnen lezen versies die geen quorum doorgevoerd.
Dit betekent dat een versie van de gegevens die worden gelezen uit een enkele secundaire ONWAAR kan worden gevorderd.
Lezen van primaire zijn natuurlijk altijd stabiel: kan nooit worden false gevorderd.

## <a name="next-steps"></a>Volgende stappen

- [Betrouwbare diensten snel starten](service-fabric-reliable-services-quick-start.md)
- [Werken met betrouwbare collecties](service-fabric-work-with-reliable-collections.md)
- [Betrouwbare diensten meldingen](service-fabric-reliable-services-notifications.md)
- [Betrouwbare Services back-up en terugzetten (noodherstel)](service-fabric-reliable-services-backup-restore.md)
- [Configuratie van betrouwbare staat beheer](service-fabric-reliable-services-configuration.md)
- [Aan de slag met API-Fabric Web Service services](service-fabric-reliable-services-communication-webapi.md)
- [Geavanceerd gebruik van de betrouwbare programmeermodel voor Services](service-fabric-reliable-services-advanced-usage.md)
- [Referentie voor ontwikkelaars voor betrouwbare collecties](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
