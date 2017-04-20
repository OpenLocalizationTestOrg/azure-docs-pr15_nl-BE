<properties
   pageTitle="Actoren en de bewaking | Microsoft Azure"
   description="In dit artikel beschrijft de diagnostische gegevens en functies van de runtime betrouwbare Service Fabric-actoren, met inbegrip van de gebeurtenissen en prestatiemeteritems uitgestoten door het controleren van prestaties."
   services="service-fabric"
   documentationCenter=".net"
   authors="abhishekram"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/05/2016"
   ms.author="abhisram"/>

# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Diagnose- en prestatiecontrole voor betrouwbare actoren
Hiermee plaatst u de runtime betrouwbare actoren [gebeurtenisbron](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen en [prestatiemeteritems](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Deze bieden inzicht in hoe de runtime wordt uitgevoerd en u helpen bij het oplossen van problemen en het controleren van de prestaties.

## <a name="eventsource-events"></a>Gebeurtenisbron gebeurtenissen
De naam van de gebeurtenisbron voor betrouwbare actoren runtime is 'Microsoft ServiceFabric Acteurs'. Gebeurtenissen van deze gebeurtenisbron weergegeven in het venster [Diagnostiek gebeurtenissen](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) wanneer de toepassing van de acteur [Foutopsporing in Visual Studio wordt](service-fabric-debugging-your-application.md).

Voorbeelden van hulpmiddelen en technologieën die helpen bij het inzamelen en/of gebeurtenisbron gebeurtenissen zijn [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) [Semantische Logging](https://msdn.microsoft.com/library/dn774980.aspx)en de [Bibliotheek van Microsoft TraceEvent](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Trefwoorden
Alle gebeurtenissen die deel uitmaken van de betrouwbare actoren gebeurtenisbron zijn gekoppeld aan een of meer trefwoorden. Hierdoor kunnen filteren van gebeurtenissen die worden verzameld. Het volgende sleutelwoord bits worden gedefinieerd.

|Bits|Beschrijving|
|---|---|
|0x1|Instellen van belangrijke gebeurtenissen op de werking van de runtime Fabric actoren om samen te vatten.|
|0x2|Set methodeaanroepen acteur beschrijven gebeurtenissen. Zie de [inleidende informatie over de actoren](service-fabric-reliable-actors-introduction.md#actors)voor meer informatie.|
|0x4|Reeks gebeurtenissen met betrekking tot acteur staat. Zie het onderwerp over het [beheer van acteur staat](service-fabric-reliable-actors-state-management.md)voor meer informatie.|
|0x8|Reeks gebeurtenissen met betrekking tot turn-based gelijktijdigheid in de actor. Zie voor meer informatie het onderwerp op de [gelijktijdigheid](service-fabric-reliable-actors-introduction.md#concurrency).|

## <a name="performance-counters"></a>Prestatiemeteritems
De betrouwbare actoren runtime definieert de volgende categorieën voor prestatiemeteritems.

|Categorie|Beschrijving|
|---|---|
|Service Fabric Actor|Items die specifiek bij Azure Service Fabric actoren, bijvoorbeeld tijd genomen acteur staat opslaan|
|Methode voor de acteur Fabric|Tellers specifieke methoden geïmplementeerd door Service Fabric actoren, bijvoorbeeld hoe vaak een actor methode wordt aangeroepen|

Elk van deze categorieën heeft een of meer items.

De [Prestatiemeter van Windows](https://technet.microsoft.com/library/cc749249.aspx) -toepassing die is standaard beschikbaar in het Windows-besturingssysteem kan worden gebruikt voor het verzamelen en weergeven van gegevens van prestatiemeteritems. [Azure diagnostische gegevens](../cloud-services/cloud-services-dotnet-diagnostics.md) is een andere optie voor het verzamelen van gegevens van prestatiemeteritems en uploaden naar Azure tabellen.

### <a name="performance-counter-instance-names"></a>Namen van prestatietellers exemplaar
Een cluster met een groot aantal services actor of actor-partities heeft een groot aantal exemplaren van acteur prestaties teller. De namen van prestatietellers exemplaar kunnen helpen bij het identificeren van de specifieke methode voor de [partitie](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) en acteur (indien van toepassing) het exemplaar van het prestatiemeteritem is gekoppeld.

#### <a name="service-fabric-actor-category"></a>Categorie Service Fabric Actor
Voor de categorie `Service Fabric Actor`, de instantienamen teller zijn in de volgende notatie:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* is de reeksaanduiding van de Service Fabric partitie-ID die door het exemplaar van het prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeks wordt gegenereerd via de [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met de specificatie van de indeling "D".

*ActorRuntimeInternalID* is de reeksaanduiding van een 64-bits integer die wordt gegenereerd door de runtime Fabric actoren voor intern gebruik. Dit is opgenomen in de naam van het exemplaar de uniekheid en conflict met andere namen van prestatietellers exemplaar voorkomen. Gebruikers moeten niet proberen te interpreteren dit gedeelte van de naam van het exemplaar.

Het volgende is een voorbeeld van een naam van het exemplaar van een item dat hoort bij de `Service Fabric Actor` categorie:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

In het bovenstaande voorbeeld `2740af29-78aa-44bc-a20b-7e60fb783264` wordt de reeksaanduiding van de Service Fabric partitie-ID, en `635650083799324046` wordt de 64-bits-ID die is gegenereerd voor de runtime interne's gebruiken.

#### <a name="service-fabric-actor-method-category"></a>Methode voor de acteur Fabric-categorie
Voor de categorie `Service Fabric Actor Method`, de instantienamen teller zijn in de volgende notatie:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* is de naam van de acteur-methode waarmee het exemplaar van het prestatiemeteritem is gekoppeld. De indeling van de naam van de methode wordt bepaald op basis van een bepaalde logica in de runtime actoren weefsel dat de leesbaarheid van de naam met beperkingen op de maximale lengte van de namen van prestatietellers exemplaar van Windows.

*ActorsRuntimeMethodId* is de reeksaanduiding van een 32-bits integer die wordt gegenereerd door de runtime Fabric actoren voor intern gebruik. Dit is opgenomen in de naam van het exemplaar de uniekheid en conflict met andere namen van prestatietellers exemplaar voorkomen. Gebruikers moeten niet proberen te interpreteren dit gedeelte van de naam van het exemplaar.

*ServiceFabricPartitionID* is de reeksaanduiding van de Service Fabric partitie-ID die door het exemplaar van het prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeks wordt gegenereerd via de [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met de specificatie van de indeling "D".

*ActorRuntimeInternalID* is de reeksaanduiding van een 64-bits integer die wordt gegenereerd door de runtime Fabric actoren voor intern gebruik. Dit is opgenomen in de naam van het exemplaar de uniekheid en conflict met andere namen van prestatietellers exemplaar voorkomen. Gebruikers moeten niet proberen te interpreteren dit gedeelte van de naam van het exemplaar.

Het volgende is een voorbeeld van een naam van het exemplaar van een item dat hoort bij de `Service Fabric Actor Method` categorie:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

In het voorbeeld hierboven, `ivoicemailboxactor.leavemessageasync` is de naam van de methode `2` is de 32-bits-ID gegenereerd voor intern gebruik van de runtime, `89383d32-e57e-4a9b-a6ad-57c6792aa521` wordt de reeksaanduiding van de Service Fabric partitie-ID, en `635650083804480486` wordt de 64-bits-ID gegenereerd voor de runtime interne's gebruiken.

## <a name="list-of-events-and-performance-counters"></a>Lijst van gebeurtenissen en prestatiemeteritems

### <a name="actor-method-events-and-performance-counters"></a>Acteur methode gebeurtenissen en prestatiemeteritems
De betrouwbare actoren runtime zendt de volgende gebeurtenissen met betrekking tot de [methoden van de actor](service-fabric-reliable-actors-introduction.md#actors).

|Naam van evenement|Gebeurtenis-ID|Niveau|Trefwoord|Beschrijving|
|---|---|---|---|---|
|ActorMethodStart|7|Uitgebreide|0x2|Actoren runtime is een actor-methode aangeroepen.|
|ActorMethodStop|8|Uitgebreide|0x2|Uitvoering van een actor-methode is voltooid. Dat wil zeggen, de runtime van asynchrone aanroep van de methode actor heeft geretourneerd en de taak die wordt geretourneerd door de methode actor is voltooid.|
|ActorMethodThrewException|9|Waarschuwing|0x3|Er is een uitzondering opgetreden tijdens de uitvoering van een actor-methode van tijdens de runtime van asynchrone aanroep van de methode actor of tijdens de uitvoering van de taak die wordt geretourneerd door de methode actor. Deze gebeurtenis geeft aan dat sommige sortering van de fout in de code actor die onderzoek moet.|

De betrouwbare actoren runtime publiceert de volgende prestatiemeters die betrekking hebben op de uitvoering van een actor-methoden.

|Naam van categorie|Naam van het item|Beschrijving|
|---|---|---|
|Methode voor de acteur Fabric|Aanroepen per seconde|Aantal keren dat de actor-methode wordt aangeroepen per seconde|
|Methode voor de acteur Fabric|Gemiddelde milliseconden per aanroep|Tijd die nodig is voor het uitvoeren van de methode voor de actor in milliseconden|
|Methode voor de acteur Fabric|Uitzonderingen verzonden per seconde|Aantal keren dat de actor methode service heeft een uitzondering per seconde|

### <a name="concurrency-events-and-performance-counters"></a>Gelijktijdigheid van gebeurtenissen en prestatiemeteritems
De betrouwbare actoren runtime zendt de volgende gebeurtenissen met betrekking tot [gelijktijdigheid](service-fabric-reliable-actors-introduction.md#concurrency).

|Naam van evenement|Gebeurtenis-ID|Niveau|Trefwoord|Beschrijving|
|---|---|---|---|---|
|ActorMethodCallsWaitingForLock|12|Uitgebreide|0x8|Deze gebeurtenis wordt geschreven aan het begin van elke nieuwe beurt in een actor. Het bevat het aantal in behandeling actor oproepen die wachten op vergrendelen voor de per-acteur die op basis van een turn gelijktijdigheid wordt afgedwongen.|

De betrouwbare actoren runtime publiceert de volgende prestatiemeters die betrekking hebben op de gelijktijdigheid.

|Naam van categorie|Naam van het item|Beschrijving|
|---|---|---|
|Service Fabric Actor|# acteur oproepen wachten actor vergrendeling|Aantal in behandeling zijnde actor gesprekken wacht om te vergrendelen voor de per-acteur die op basis van een turn gelijktijdigheid wordt afgedwongen|
|Service Fabric Actor|Gemiddelde milliseconden per vergrendeling wachten|Tijd (in milliseconden) de vergrendeling per actor die wordt afgedwongen op basis van een turn gelijktijdigheid te verkrijgen|
|Service Fabric Actor|Gemiddelde milliseconden actor vergrendeling die wordt veroorzaakt|Tijd (in milliseconden) die de per acteur vergrendeling|

### <a name="actor-state-management-events-and-performance-counters"></a>Gebeurtenissen voor acteur staat en prestatiemeteritems
De betrouwbare actoren runtime zendt de volgende [actor statusbeheer](service-fabric-reliable-actors-state-management.md)-gebeurtenissen.

|Naam van evenement|Gebeurtenis-ID|Niveau|Trefwoord|Beschrijving|
|---|---|---|---|---|
|ActorSaveStateStart|10|Uitgebreide|0x4|Actoren runtime is de acteur status opslaan.|
|ActorSaveStateStop|11|Uitgebreide|0x4|Actoren runtime is klaar met het opslaan van de acteur staat.|

De betrouwbare actoren runtime publiceert de volgende prestatiemeters voor statusbeheer actor.

|Naam van categorie|Naam van het item|Beschrijving|
|---|---|---|
|Service Fabric Actor|Gemiddelde milliseconden per opslaan staat bewerking|Tijd staat acteur opslaan in milliseconden|
|Service Fabric Actor|Gemiddelde milliseconden per laadbewerking staat|De tijd staat actor in milliseconden laden|

### <a name="events-related-to-actor-replicas"></a>Gebeurtenissen met betrekking tot acteur replica 's
De betrouwbare actoren runtime zendt de volgende gebeurtenissen met betrekking tot [acteur replica's](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateful-actors).

|Naam van evenement|Gebeurtenis-ID|Niveau|Trefwoord|Beschrijving|
|---|---|---|---|---|
|ReplicaChangeRoleToPrimary|1|Informatieve|0x1|Acteur replica gewijzigd in primaire rol. Dit houdt in dat de actoren voor deze partitie binnen deze replica wordt gemaakt.|
|ReplicaChangeRoleFromPrimary|2|Informatieve|0x1|Acteur replica gewijzigd in niet-primaire rol. Dit houdt in dat de actoren voor deze partitie niet meer in deze replica worden gemaakt. Er zijn geen nieuwe aanvragen zullen worden geleverd op actoren al binnen deze replica wordt gemaakt. De actoren zullen worden vernietigd nadat alle aanvragen in uitvoering zijn voltooid.|

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Actor-activering en deactivering gebeurtenissen en prestatiemeteritems
De betrouwbare actoren runtime zendt de volgende gebeurtenissen met betrekking tot [acteur activeren en deactiveren](service-fabric-reliable-actors-lifecycle.md).

|Naam van evenement|Gebeurtenis-ID|Niveau|Trefwoord|Beschrijving|
|---|---|---|---|---|
|ActorActivated|5|Informatieve|0x1|Een actor is geactiveerd.|
|ActorDeactivated|6|Informatieve|0x1|Een actor is gedeactiveerd.|

De betrouwbare actoren runtime publiceert de volgende prestatiemeters die betrekking hebben op de actor activeren en deactiveren.

|Naam van categorie|Naam van het item|Beschrijving|
|---|---|---|
|Service Fabric Actor|Gemiddelde OnActivateAsync milliseconden|Tijd die nodig is voor het uitvoeren van de methode OnActivateAsync in milliseconden|

### <a name="actor-request-processing-performance-counters"></a>Prestatiemeteritems actor verwerken
Als een client een methode via een proxy actor object aanroept, wordt in een bericht wordt verzonden via het netwerk met de acteur-service. De service verwerkt het request-bericht en stuurt een antwoord naar de client. De betrouwbare actoren runtime publiceert de volgende prestatiemeters die betrekking hebben op de verwerking van acteur.

|Naam van categorie|Naam van het item|Beschrijving|
|---|---|---|
|Service Fabric Actor|# uitstaande aanvragen|Het aantal aanvragen dat is verwerkt in de service|
|Service Fabric Actor|Gemiddelde milliseconden per aanvraag|Tijd (in milliseconden) die door de service voor het verwerken van een aanvraag|
|Service Fabric Actor|Gemiddelde milliseconden voor aanvraag deserialisatie|Tijd (in milliseconden) deserialize actor-request-bericht wanneer het is ontvangen door de service|
|Service Fabric Actor|Gemiddelde milliseconden voor serialisatie antwoord|Tijd (in milliseconden) het antwoordbericht actor bij de service serialiseren voordat het antwoord naar de client wordt verzonden|

## <a name="next-steps"></a>Volgende stappen
 - [Hoe betrouwbaar actoren het platform Fabric-Service gebruiken](service-fabric-reliable-actors-platform.md)
 - [Acteur API-documentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Voorbeeld van code](https://github.com/Azure/servicefabric-samples)
