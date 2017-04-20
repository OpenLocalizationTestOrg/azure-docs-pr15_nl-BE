<properties
    pageTitle="Modeling Multitenancy in Azure zoeken | Microsoft Azure | De zoekservice hosted cloud"
    description="Informatie over het algemene ontwerppatronen voor multitenant SaaS-toepassingen tijdens het gebruik van Azure zoeken."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/26/2016"
    ms.author="ashmaka"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Patronen voor multitenant SaaS-toepassingen en Azure Search ontwerpen

Een multitenant-toepassing is een die biedt dezelfde mogelijkheden en services op elk gewenst aantal huurders die u niet kunt zien of de gegevens van een andere huurder delen. Dit document wordt besproken huurder isolatie strategieën voor multitenant toepassingen die zijn ontwikkeld met Azure Search.

## <a name="azure-search-concepts"></a>Azure Search concepten
Als een oplossing zoeken als een service kunnen Azure Search ontwikkelaars zoeken rijke ervaringen toevoegen aan toepassingen zonder beheer van de infrastructuur of steeds een deskundige in de zoekresultaten. Gegevens worden geüpload naar de service en vervolgens opgeslagen in de cloud. Met behulp van eenvoudige aanvragen tot de zoek-API Azure, kunnen de gegevens vervolgens worden gewijzigd en doorzocht. In [dit artikel](http://aka.ms/whatisazsearch)vindt u een overzicht van de service. Voordat ontwerppatronen bespreken, is het belangrijk dat u enkele basisbegrippen in Azure zoeken.

### <a name="search-services-indexes-fields-and-documents"></a>Services, indexen, velden en documenten zoeken
Als u zoeken in Azure, is een een _zoekservice_geabonneerd. Als u gegevens naar Azure Search is geüpload, wordt deze opgeslagen in een _index_ binnen de zoekservice. Er zijn een aantal indexen in een enkele service. Voor het gebruik van de bekende concepten van databanken, kan de search-service worden vergeleken met een database terwijl de indexen in een service kunnen worden vergeleken met tabellen in een database.

Elke index in een zoekservice heeft een eigen schema dat is gedefinieerd door een aantal aanpasbare _velden_. Gegevens worden toegevoegd aan een Azure-zoekindex in de vorm van afzonderlijke _documenten_. Elk document moet worden geüpload naar een bepaalde index en het schema van die index moet passen. Bij het zoeken naar gegevens met Azure zoeken, zoeken in volledige tekst query's uitgegeven tegen een bepaalde index.  Als u wilt vergelijken deze concepten die van een database, velden kunnen worden vergeleken met de kolommen in een tabel en documenten kunnen worden vergeleken met rijen.

### <a name="scalability"></a>Schaalbaarheid
Alle Azure Search-service in de standaard [prijzen laag](https://azure.microsoft.com/pricing/details/search/) kunt schalen in twee dimensies: opslag en beschikbaarheid.
* _Partities_ kunnen worden toegevoegd aan het verhogen van de opslag van een zoekservice.
* _Replica's_ kunnen worden toegevoegd aan een service te verhogen van de doorvoer van aanvragen die een search-service kan verwerken.

Toevoegen en verwijderen partities en replica's op kunt de capaciteit van de zoekservice te groeien met de hoeveelheid gegevens en de vereisten van de toepassing verkeer. Om een zoekservice Lees [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)te bereiken, moet de twee replica's. Om een service te bereiken een lezen / schrijven- [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), moet drie replica's.


### <a name="service-and-index-limits-in-azure-search"></a>Service- en grenzen in Azure zoeken
Er zijn een paar verschillende [lagen prijzen](https://azure.microsoft.com/pricing/details/search/) in Azure Search, elk van de niveaus heeft verschillende [limieten en quota](search-limits-quotas-capacity.md). Sommige van deze limieten zijn op het serviceniveau, sommige zijn op niveau van de index, en andere op de partitie-niveau.


|                                  | Basic     | Standard1   | Standard2   | Standard3   | HD Standard3  |
|----------------------------------|-----------|-------------|-------------|-------------|---------------|
| Maximale replica's per Service     | 3         | 12          | 12          | 12          | 12            |
| Maximale partities per Service   | 1         | 12          | 12          | 12          | 1             |
| Maximale Zoeken eenheden (replica's * partities) per Service | 3         | 36          | 36          | 36          | 36 (max. 3 partities)            |
| Maximale documenten per Service    | 1 miljoen | 180 miljoen | 720 miljoen | 1,4 miljard | 600 miljoen   |
| Maximale opslag per Service      | 2 GB      | 300 GB      | 1,2 TB      | 2.4 TB      | 600 GB        |
| Maximale documenten per partitie  | 1 miljoen | 15 miljoen  | 60 miljoen  | 120 miljoen | 200 miljoen   |
| Maximale opslag per partitie    | 2 GB      | 25 GB       | 100 GB      | 200 GB      | 200 GB        |
| Maximale indexen per Service      | 5         | 50          | 200         | 200         | 3000 (max 1000 indexen/partitie)          |


#### <a name="s3-high-density"></a>S3 Hoge dichtheid '
Er is een optie voor de hoge dichtheid (HD) is speciaal ontworpen voor scenario's voor multitenant in Azure Search van S3 prijzen laag. In veel gevallen is het noodzakelijk ter ondersteuning van een groot aantal kleinere huurders onder een enkele service om te profiteren van de voordelen van vereenvoudiging en kostenbesparing.

S3 HD kunnen veel kleine indexen moeten worden verpakt onder het beheer van een enkele search-service door de mogelijkheid voor het schalen van de indexen die partities voor de mogelijkheid om meer indexen in een enkele service handel.

Concrete invulling te geven, zijn een S3-service kan tussen 1 en 200 indexen die samen tot 1,4 miljard documenten kunnen beschikken. Een HD S3 aan de andere kant kunnen afzonderlijke indexen alleen gaan 1 miljoen documenten, maar maximaal 1000 indexen per partitie (tot 3000 per service) met een totale document telling van 200 miljoen per partitie kan verwerken (maximaal 600 miljoen per service).



## <a name="considerations-for-multitenant-applications"></a>Overwegingen voor multitenant toepassingen
Multitenant toepassingen distribueren van resources tussen de huurders effectief behoud van sommige niveau van privacy tussen de verschillende huurders. Er zijn een paar overwegingen bij het ontwerpen van de architectuur voor een dergelijke toepassing:

* _Isolatie pachters:_ Ontwikkelaars van toepassingen moeten passende maatregelen om ervoor te zorgen dat er geen huurders onbevoegde of toegang tot de gegevens van andere huurders ongewenste. Huurder isolatie strategieën moeten buiten het perspectief van de persoonlijke levenssfeer gegevens effectief beheer van gedeelde bronnen en de bescherming van de luide buren.
* _Kosten van de resource van de wolk:_ Net als bij andere toepassingen moeten softwareoplossingen kosten concurrerende als onderdeel van een multitenant toepassing blijven.
* _Bewerkingen te vereenvoudigen:_ Bij het ontwikkelen van een multitenant-architectuur, is de impact op de bedrijfsactiviteiten en de complexiteit van de toepassing een belangrijke overweging. Azure Search is een [SLA 99,9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* _Global footprint:_ Multitenant toepassingen mogelijk dienen effectief huurders die verdeeld zijn over de hele wereld.
* _Schaalbaarheid:_ Toepassingsontwikkelaars hoeven te overwegen hoe ze afstemmen met handhaving van een voldoende laag niveau van complexiteit van de toepassing en het ontwerpen van de toepassing voor het schalen van huurders en de grootte van de gegevens en de werkbelasting huurders.

Azure Search biedt een paar grenzen die kunnen worden gebruikt voor het isoleren van huurders gegevens en werkbelasting.

## <a name="modeling-multitenancy-with-azure-search"></a>Multitenancy modellering met Azure Search
In het geval van een scenario multitenant, ontwikkelaar van de toepassing een of meer diensten worden verbruikt en delen hun huurders tussen services of indexen. Azure Search heeft enkele algemene patronen bij het modelleren van een multitenant scenario:

1. _Index per huurder:_ Elke huurder heeft een eigen index in een zoekservice die wordt gedeeld met andere huurders.
1. _Service per huurder:_ Elke huurder heeft een eigen speciale Azure Search-service biedt hoogste niveau van gegevens en werkbelasting scheiding.
1. _Combinatie van beide:_ Grotere, meer actieve huurders krijgen speciale services terwijl kleinere huurders zijn afzonderlijke indexen in de gedeelde services toegewezen.

## <a name="1-index-per-tenant"></a>1. per huurder indexeren
![Een portrayal van het model voor de index per huurder](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

In een model index per huurder innemen meerdere huurders een Azure Search service waar elke huurder heeft een eigen index.

Huurders bereiken gegevensisolatie omdat alle aanvragen zoeken en document-bewerkingen op het indexniveau van een in Azure Search worden uitgegeven. In de toepassingslaag is het bewustzijn van de noodzaak om de verschillende huurders verkeer naar de juiste indexen tijdens het beheren van bronnen op het serviceniveau van de ook over alle huurders.

Een belangrijk kenmerk van de index per huurder model is de mogelijkheid voor de ontwikkelaar van de toepassing om de capaciteit van een zoekservice tussen huurders van de toepassing oversubscribe. Als de huurder een ongelijke verdeling van de werkbelasting, kan de optimale combinatie van huurders worden verdeeld over een zoekservice indexen aangepast aan een aantal zeer actieve, resource-intensieve huurders terwijl tegelijkertijd een lange staart van huurders minder actieve. De verhouding is het onvermogen van het model voor situaties waar elke huurder gelijktijdig zeer actief is.

De index per huurder model vormt de basis voor een model van variabele kosten, waar een hele Azure Search service aanvang is gekocht en vervolgens gevuld met huurders. Hierdoor kunnen niet-gebruikte capaciteit van de proeven en de gratis accounts worden aangewezen.

Voor toepassingen met een mondiale voetafdruk, de index per huurder model mogelijk niet de meest efficiënte. Als de huurders van de toepassing zijn verdeeld over de hele wereld, wellicht een afzonderlijke service voor elke regio die kosten voor elk van hen kan dupliceren.

De schaal van de afzonderlijke indexen en het totale aantal indexen kan Azure Search groeien. Als de juiste prijs wordt laag gekozen partities en replica's kunnen worden toegevoegd aan de gehele search-service wanneer een afzonderlijke index binnen de service te groot is voor opslag of verkeer.

Als het totale aantal indexen te voor een service groot is een andere service worden ingericht ten behoeve van de nieuwe huurder. Als u indexen hebt als nieuwe services worden toegevoegd tussen de zoekservices worden verplaatst, is de gegevens uit de index handmatig worden gekopieerd uit een index naar de andere als Azure Search is niet toegestaan voor een index moet worden verplaatst.


## <a name="2-service-per-tenant"></a>2. Service per huurder
![Een portrayal van het model service per huurder](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

Elke huurder heeft een architectuur service per huurder een eigen zoekservice.

In dit model realiseert de toepassing het maximumgehalte aan isolatie voor de huurders. Elke service heeft speciale opslag en doorvoer voor de verwerking van zoekopdracht als aparte API-sleutels.

De service per huurder model is een daadwerkelijke keuze voor toepassingen waarbij elke huurder heeft een grote footprint of de werkbelasting variëren pachter huurder als resources niet worden gedeeld door verschillende huurders werkbelasting.

Een service per huurder model biedt ook het voordeel van een kostprijsmodel van voorspelbare en vaste. Er is geen aanvang investering in een volledige zoekservice totdat er een huurder aan te vullen, maar de kosten per huurder hoger dan een model index per huurder is.

De service per huurder model is een efficiënte keuze voor toepassingen met een globale voetafdruk. Met huurders geografisch verspreid kan het gemakkelijk van elke huurder service in de desbetreffende regio.

De uitdagingen in de schaal van dit patroon ontstaan wanneer individuele huurders langzamerhand hun service. Azure Search ondersteunt momenteel geen upgrade van de prijzen laag van een zoekservice, zodat alle gegevens zou moeten handmatig worden gekopieerd naar een nieuwe service.

## <a name="3-mixing-both-models"></a>3. combinatie van beide modellen
Een ander patroon voor het modelleren van multitenancy is zowel index per huurder service per huurder strategieën te mengen.

Door menging van de twee patronen, grootste huurders van de toepassing in beslag kunnen nemen specifieke services tijdens de lange staart van huurders minder actieve, kleinere indexen in een gedeelde service kan innemen. Dit model zorgt ervoor dat de grootste huurder consistent hoge prestaties van de service bij het beveiligen van de kleinere huurders van een luide buren hebben.

Uitvoering van deze strategie is de prognose voorspellen welke huurder vereist een speciale service versus een index in een gedeelde service afhankelijk. Met de noodzaak voor het beheren van deze modellen multitenancy verhoogt de complexiteit van de toepassing.

## <a name="achieving-even-finer-granularity"></a>Zelfs weer specifieker te bereiken
De bovenstaande ontwerppatronen multitenant scenario's in Azure Search model wordt uitgegaan van een uniform scope waar elke huurder een volledig exemplaar van een toepassing is. Toepassingen kunnen echter soms veel kleinere bereiken verwerken.

Als service per huurder en index per huurder modellen niet voldoende kleine bereiken, is het mogelijk om een index te bereiken een nog kleinere indeling mate van granulatie.

Als u een enkele index gedragen zich anders voor verschillende client eindpunten, kan een veld worden toegevoegd aan een index waarmee wordt aangegeven dat een bepaalde waarde voor elke client mogelijk. Elke keer dat een client Azure Search opvragen of wijzigen van een index, roept geeft de code van de clienttoepassing de juiste waarde voor dat veld met Azure van [filter](https://msdn.microsoft.com/library/azure/dn798921.aspx) zoekmogelijkheden bij query's.

Deze methode kan worden gebruikt om de functionaliteit van afzonderlijke accounts, aparte machtigingsniveaus, bereiken en zelfs volledig afzonderlijke toepassingen.

## <a name="next-steps"></a>Volgende stappen
Azure Search is een aantrekkelijke keuze voor veel toepassingen, [meer lezen over de krachtige mogelijkheden van de service](http://aka.ms/whatisazsearch). Bij de beoordeling van de verschillende ontwerppatronen voor multitenant toepassingen, kunt u de [verschillende niveaus van prijzen](https://azure.microsoft.com/pricing/details/search/) en de respectieve [service beperkt](search-limits-quotas-capacity.md) tot beste maat Azure Search aanpassen aan de werkbelasting van toepassingen en de architectuur van elke omvang.

Vragen over Azure Search en multitenant-scenario's kunnen worden gericht aan azuresearch_contact@microsoft.com.
