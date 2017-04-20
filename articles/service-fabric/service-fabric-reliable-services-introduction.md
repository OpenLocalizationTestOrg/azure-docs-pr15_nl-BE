<properties
   pageTitle="Overzicht van het programmeermodel Service Fabric betrouwbare Service | Microsoft Azure"
   description="Informatie over Service-Fabric betrouwbare Service programmeermodel en schrijven van uw eigen diensten aan de slag."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor="vturecek; mani-ramaswamy"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="masnider;vturecek"/>

# <a name="reliable-services-overview"></a>Betrouwbare Services-overzicht
Azure Service Fabric vereenvoudigt het schrijven en stateless en stateful betrouwbare Services te beheren. Dit document zal praten over:

- De betrouwbare diensten programmeermodel voor stateless en stateful-services.
- De keuzes die u maken moet bij het schrijven van een betrouwbare Service.
- Sommige scenario's en voorbeelden van het gebruik van betrouwbare diensten en hoe deze zijn geschreven.

Betrouwbare Services is een van de programmeermodellen op Fabric-Service beschikbaar. Zie voor meer informatie over het programmeermodel betrouwbare actoren, [Inleiding tot betrouwbare Service Fabric-actoren](service-fabric-reliable-actors-introduction.md).

Service stof, een service bestaat uit de code van de toepassing, configuratie en (optioneel) staat.

Fabric-service beheert de levensduur van de diensten, van de inrichting en implementatie via upgrade en verwijdering via [Service Fabric Toepassingsbeheer](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Wat zijn betrouwbare diensten?
Betrouwbare Services biedt u een eenvoudige, krachtige, op het hoogste niveau programmeermodel waarmee u express wat belangrijk is voor uw toepassing is. Met de betrouwbare programmeermodel voor Services, geniet u van:

- Voor stateful-services kunt het programmeermodel betrouwbare diensten u uw staat recht binnen de service consistent en betrouwbaar worden opgeslagen met behulp van betrouwbare collecties. Dit is een eenvoudige set van maximaal beschikbare verzameling klassen die vertrouwd zijn voor iedereen die in C# collecties heeft gebruikt. Traditioneel nodig services externe systemen voor het beheer van betrouwbare staat. Met betrouwbare collecties, kunt u uw staat naast de compute opslaan met de dezelfde hoge betrouwbaarheid en beschikbaarheid die ben je kunt verwachten van hoge beschikbaarheid externe winkels, en met de extra latentie verbeteringen die samen zoeken naar de compute en staat.

- Een eenvoudig model voor uw eigen code uitvoert die lijkt op de modellen die u om te gebruikt programmeren. De code is een goed gedefinieerde ingangspunt en levenscyclus van eenvoudig te beheren.

- Een communicatiemodel pluggable. Gebruik het transport van uw keuze, bijvoorbeeld HTTP met [Web API](service-fabric-reliable-services-communication-webapi.md), WebSockets, aangepaste protocollen voor TCP, enz. Betrouwbare Services bieden een uitstekende out-of-the-box-opties die u kunt gebruiken, of kunt u uw eigen.

## <a name="what-makes-reliable-services-different"></a>Wat maakt betrouwbare Services verschillende?
Betrouwbare diensten Service stof verschilt van de services die u hebt gemaakt voordat. Fabric-service biedt betrouwbaarheid, beschikbaarheid, consistentie en schaalbaarheid.  

- **Betrouwbaarheid**: de service blijft ook in onbetrouwbaar omgevingen waar uw machines kunnen mislukken of druk op de netwerkverbinding.

- **Beschikbaarheid**--uw service is bereikbaar en reageren. (Dit betekent niet dat er geen services kunnen niet worden gevonden of bereikt vanaf buiten.)

- **Schaalbaarheid**- Services worden ontkoppeld van de specifieke hardware en ze kunnen groter of kleiner als door het toevoegen of verwijderen van hardware of virtuele bronnen nodig. Services zijn eenvoudig gepartitioneerd (met name in het geval stateful) om ervoor te zorgen dat onafhankelijke delen van de service kunnen schalen en op fouten van onafhankelijk van elkaar reageren. Tot slot moedigt Fabric Service services lichtgewicht door duizenden services binnen één proces worden ingericht zodat in plaats van verplicht stellen of programmathreads hele OS exemplaren op één exemplaar van een bepaalde werkbelasting.

- **Consistentie**- de gegevens zijn opgeslagen in deze service kan worden gegarandeerd consistent (dit geldt alleen voor stateful services - meer op dit later)

## <a name="service-lifecycle"></a>Service lifecycle
Of uw service stateful of stateless, voorzien betrouwbare Services in een eenvoudige lifecycle waarmee u snel in uw code aansluit en aan de slag.  Er zijn maar een of twee methoden die u nodig hebt om uw service bedrijf te implementeren.

- **CreateServiceReplicaListeners/CreateServiceInstanceListeners** - dit is waar de service definieert de communicatie-stack die zij wil gebruiken. De stack communicatie, zoals [Web-API](service-fabric-reliable-services-communication-webapi.md), is wat definieert de luisterende eindpunt of eindpunten voor de service (hoe clients, komt deze). Ook wordt gedefinieerd hoe de berichten die, einde verschijnen van de interactie met de rest van de code.

- **RunAsync** - dit is waar de bedrijfslogica in uw service wordt uitgevoerd. De annulering token die wordt aangeboden is een signaal voor wanneer dat werk moet worden gestopt. Bijvoorbeeld, als u een service die moet voortdurend berichten uit een wachtrij betrouwbare halen en deze te verwerken, is dit waar dat werk zou gebeuren.

### <a name="service-startup"></a>Starten van de service

De belangrijkste gebeurtenissen in de levenscyclus van een betrouwbare Service zijn:

1. Het object (het ding dat is afgeleid van de service van stateless of stateful service) is gebouwd.

2. De `CreateServiceReplicaListeners` / `CreateServiceInstanceListeners` methode wordt aangeroepen, zodat de service een kans om terug te keren communicatie-listeners voor een of meer van zijn keuze.
  - Houd er rekening mee dat dit optioneel, is Hoewel de meeste services zal sommige eindpunt rechtstreeks worden blootgesteld.

3. Nadat de listeners communicatie hebt gemaakt, wordt het geopend.
  - Communicatie-listeners hebt u een methode met de naam `OpenAsync()`, die op dit moment wordt genoemd en die resulteert in het adres luisteren voor de service. Als uw betrouwbare Service een van de ingebouwde ICommunicationListeners gebruikt, is dit voor u verwerkt.

4. Zodra de listener communicatie geopend is, de `RunAsync()` heet de methode van de belangrijkste service.
  - Houd er rekening mee dat `RunAsync()` is optioneel. Als de service al haar werkzaamheden rechtstreeks in reactie op alleen is roept, het is niet nodig voor het implementeren van `RunAsync()`.

### <a name="service-shutdown"></a>Afsluiten van de service

Als de service wordt afgesloten (om te worden verwijderd, bijgewerkt of verplaatste) de aanroep-volgorde is gespiegeld: eerst de annulering token bezit van `RunAsync()` is geannuleerd. Klik `CloseAsync()` op de communicatie-listeners wordt genoemd.

Er zijn een paar belangrijke zaken met betrekking tot afsluiten voor stateful-services:

- Fabric-service wordt niet een andere replica van uw service naar de primaire status tot bevordering van `CloseAsync` en `RunAsync` zijn geretourneerd. Als u een listener ingebouwde communicatie met de `CloseAsync` methode is verwerkt voor u.

- Hoewel er geen beperking bij het retourneren van deze methoden, kunt u onmiddellijk de mogelijkheid kunt verliezen u schrijven naar betrouwbare collecties en daarom werkelijke hoeveelheid werk niet voltooien. Het wordt aanbevolen dat u zo snel mogelijk na ontvangst van het verzoek om annulering terugkeren.

## <a name="example-services"></a>Voorbeeld van de diensten
Dit programmeermodel weten, laten we snel op twee verschillende services om te zien hoe deze stukken in elkaar passen.

### <a name="stateless-reliable-services"></a>Stateless betrouwbare diensten
Een stateless service is een waar is letterlijk geen staat binnen de service blijven of de staat die zich volledig beschikbaar is en u hoeft synchronisatie, replicatie, persistentie of hoge beschikbaarheid.

Neem bijvoorbeeld een rekenmachine die heeft geen geheugen en alle voorwaarden en bewerkingen uitvoeren in één keer ontvangt.

In dit geval kan de RunAsync() van de service niet leeg zijn, omdat er geen achtergrond taak-bewerkingen die de service moet doen. Wanneer de service Rekenmachine wordt gemaakt, wordt een ICommunicationListener die een eindpunt luisteren op een poort wordt geopend (bijvoorbeeld [Web API](service-fabric-reliable-services-communication-webapi.md)) geretourneerd. Dit eindpunt luisteren zal aansluiten op de verschillende methoden (voorbeeld: 'Toevoegen (n1, n2)') waarmee u de openbare API van de Rekenmachine.

Wanneer een aanroep wordt uitgevoerd vanaf een client, de juiste methode wordt aangeroepen en de Rekenmachine-service voert de bewerkingen op de gegevens die u verstrekt en wordt het resultaat. Een staat worden niet opgeslagen.

Geen interne status niet op te slaan kunt in dit voorbeeld van de Rekenmachine u heel eenvoudig. Maar de meeste services niet echt stateless. In plaats daarvan extern zij hun status naar een andere winkel. (Bijvoorbeeld een web app dat gebaseerd is op het houden van de sessiestatus in een reservekopie of cache is niet volledig stateless.)

Een algemeen voorbeeld van hoe de stateless services worden gebruikt in Service-structuur is als front-end dat toegang biedt tot de publieke API voor een webtoepassing. De front-end-service vervolgens gesprekken voert stateful services voor het voltooien van een aanvraag van de gebruiker. In dit geval worden aanroepen van clients doorgestuurd naar een bekende poort, bijvoorbeeld 80, waar de stateless service luistert. Deze stateless service ontvangt de aanroep en wordt bepaald of de aanroep afkomstig van een vertrouwde partij is, alsmede welke service bestemd voor.  Vervolgens de stateless service stuurt de oproep naar de correcte partitie van de stateful-service en wacht op een antwoord. Wanneer de service stateless een antwoord ontvangt, antwoorden deze terug naar de oorspronkelijke client.

### <a name="stateful-reliable-services"></a>Stateful betrouwbare diensten
Een stateful service is dat een gedeelte van de staat blijven consistent en aanwezig zijn om de service aan functie moet hebben. U kunt een service die voortdurend berekent een huidig gemiddelde van een waarde op basis van de updates die worden ontvangen. U doet dit door moet de huidige set van binnenkomende aanvragen verwerken, evenals het gemiddelde van de huidige moet hebben. Elke service die worden opgehaald, worden verwerkt en worden gegevens opgeslagen in een externe opslag (zoals een Azure blob of tabel winkel vandaag) is stateful. Het NET zorgt ervoor dat de staat in het archief met externe staat.

De meeste services opslaan vandaag de staat extern, omdat de externe opslag is wat biedt betrouwbaarheid, beschikbaarheid, schaalbaarheid en consistentie voor die staat. In Service-Fabric zijn niet stateful services vereist voor het opslaan van de staat extern; Fabric-service zorgt voor de eisen voor zowel de code als de status van de service.

Stel dat we willen schrijven een service waarmee aanvragen voor een reeks van conversies die moeten worden uitgevoerd op een afbeelding en de afbeelding die moet worden geconverteerd.  Voor deze service, levert deze communicatie listener (Stel Web API) dat een communicatiepoort wordt geopend en kan indienen via een API, zoals `ConvertImage(Image i, IList<Conversion> conversions)`. In deze API, kan de service de gegevens en de aanvraag opslaan in een wachtrij voor betrouwbare en sommige token vervolgens terug naar de client zodat deze kan bijhouden van de aanvraag (Aangezien de aanvragen enige tijd duren kunnen).

In deze service kan RunAsync complexer worden. De service kan een lus binnen de RunAsync die bij het aanvragen van IReliableQueue wordt getrokken, de conversies die worden uitgevoerd en worden de resultaten opgeslagen in IReliableDictionary, hebben, zodat wanneer de client weer terug, ze kunnen de geconverteerde afbeeldingen. Om ervoor te zorgen dat zelfs als er iets niet aan de afbeelding niet verloren gaan, deze betrouwbare Service zou worden getrokken uit de wachtrij, de conversies uitvoeren en het resultaat opslaan in een transactie. In dit geval wordt het bericht daadwerkelijk is verwijderd uit de wachtrij alleen en de resultaten worden opgeslagen in de woordenlijst resultaat als de conversie voltooid is. Als er iets niet in het midden (zoals de computer die op dit exemplaar van de code wordt uitgevoerd), blijft de aanvraag in de wachtrij moeten opnieuw worden verwerkt.

Wat te weten over deze service is dat het klinkt als een normale .NET service. Het enige verschil is dat de gegevensstructuren worden gebruikt (IReliableQueue en IReliableDictionary) worden geleverd door de Service Fabric, en dus worden gedaan zeer betrouwbare, beschikbare en consistent.

## <a name="when-to-use-reliable-services-apis"></a>Wanneer gebruikt u betrouwbare Services API 's
Als een van de volgende uw applicaties-service kenmerkt, moet u betrouwbare Services API's overwegen:

- U moet toepassingen over meerdere van de staat (bijvoorbeeld orders en orderregelartikelen).

- De status van uw toepassing kan natuurlijk als betrouwbare woordenboeken en wachtrijen worden gemodelleerd.

- Uw status moet beschikbaar zijn zeer lage latentie toegang.

- Uw toepassing moet de gelijktijdigheid of de granulatie van transactionele bewerkingen beheren via een of meer betrouwbare collecties.

- Wilt u de communicatie beheren of een partitionering voor uw service beheren.

- De code moet een free-threaded runtimeomgeving.

- Uw toepassing moet dynamisch maken of vernietigen van betrouwbare woordenlijsten of wachtrijen in runtime.

- Moet u via programmering bepalen geleverde Service Fabric back-up maken en terugzetten van functies voor de status van uw service *.

- Uw toepassing moet het wijzigingsoverzicht voor de eenheden van de staat * onderhouden.

- U wilt ontwikkelen of verbruiken staat derde partij ontwikkeld, aangepaste providers *.

> [AZURE.NOTE] * Functies die beschikbaar zijn op de algemene beschikbaarheid van SDK.


## <a name="next-steps"></a>Volgende stappen
+ [Betrouwbare diensten snel starten](service-fabric-reliable-services-quick-start.md)
+ [Betrouwbare diensten gebruik van geavanceerde](service-fabric-reliable-services-advanced-usage.md)
+ [Het programmeermodel betrouwbare actoren](service-fabric-reliable-actors-introduction.md)
