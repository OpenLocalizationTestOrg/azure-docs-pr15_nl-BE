<properties 
    pageTitle="Algemene DocumentDB use-cases | Microsoft Azure" 
    description="Meer informatie over de bovenste vijf gevallen gebruiken voor DocumentDB: gebruiker gegenereerde inhoud, logboekregistratie catalogusgegevens, gebruikersgegevens voorkeuren en Internet dingen (IoT)." 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>Algemene DocumentDB use-cases
Dit artikel bevat een overzicht van de verschillende gemeenschappelijke use-cases voor DocumentDB.  De aanbevelingen in dit artikel dienen als uitgangspunt tijdens het ontwikkelen van uw toepassing met DocumentDB.   

Na het lezen van dit artikel, zult u de volgende vragen beantwoorden: 
 
- Wat zijn de gemeenschappelijke use-cases voor DocumentDB?
- Wat zijn de voordelen van het gebruik van DocumentDB als een gebruiker gegenereerde inhoud opslaan?
- Wat zijn de voordelen van het gebruik van DocumentDB als een catalogusgegevens opslaan?
- Wat zijn de voordelen van het gebruik van DocumentDB als een logboek opslaan?
- Wat zijn de voordelen van het gebruik van DocumentDB als een gebruiker voorkeuren opslaan?
- Wat zijn de voordelen van het gebruik van DocumentDB als gegevensarchief voor systemen met Internet dingen (IoT)?

## <a name="common-use-cases-for-documentdb"></a>Algemene use-cases voor DocumentDB
Azure DocumentDB is een algemene NoSQL-database die wordt gebruikt in een breed scala van toepassingen en use-cases. Het is een goede keuze voor elke toepassing die lage responstijden volgorde van milliseconden nodig en moet snel worden geschaald. Hier volgen enkele kenmerken van de DocumentDB die zeer geschikt voor toepassingen met hoge prestaties.

- DocumentDB partities standaard van uw gegevens voor hoge beschikbaarheid en schaalbaarheid.
- De DocumentDB heeft back SSD opslag met lage latentie volgorde van milliseconde responstijden.
- DocumentDB van ondersteunt consistentie niveaus zoals uiteindelijk, sessies en begrensd staleness voor lage kosten voor prestatie-verhouding. 
- DocumentDB is een flexibele data-vriendelijke prijsstelling model meter opslag en doorvoer onafhankelijk van elkaar.
- DocumentDB van gereserveerde doorvoer model kunt u denkt dat het aantal leest/schrijft in plaats van CPU/geheugen/IO's / s van de onderliggende hardware.
- DocumentDB van ontwerp kunt die u aan de enorme aanvraag volumes in de volgorde van aanvragen per dag miljarden aanpassen.

Deze kenmerken zijn vooral nuttig wanneer het gaat om web, mobiel, games en IoT-toepassingen die moeten lage responstijden en moeten voor het verwerken van lees- en schrijfbewerkingen enorme hoeveelheden. 

## <a name="user-generated-content"></a>Gegenereerde gebruikersinhoud
Een gemeenschappelijke use-case voor DocumentDB is om op te slaan en gebruiker query inhoud (UGC) voor het web en mobiele toepassingen, met name sociale mediatoepassingen gegenereerd.  Enkele voorbeelden van UGC zijn chat-sessies, tweets, blogberichten, beoordelingen en opmerkingen.  De UGC in sociale mediatoepassingen is vaak een combinatie van vrije tekst, eigenschappen, labels en relaties die niet zijn begrensd door stijve structuur.   

Inhoud, zoals Chat-sessies, opmerkingen en berichten kunnen worden opgeslagen in de DocumentDB zonder transformaties of complexe objecten aan lagen relationele gegevens.  Eigenschappen kunnen worden toegevoegd of gewijzigd gemakkelijk om te voldoen aan de vereisten zoals ontwikkelaars doorlopen de toepassingscode dus snelle ontwikkeling te bevorderen.  

Toepassingen die kunnen worden geïntegreerd met verschillende sociale netwerken moeten reageren op veranderende schema's van deze netwerken.  Als de gegevens automatisch in het DocumentDB standaard is geïndexeerd, kan gegevens op elk gewenst moment worden opgevraagd.  Deze toepassingen hebben dus de flexibiliteit om op te halen van de uitstekende delen aan de hand van hun respectieve behoeften.       

Veel van de sociale toepassingen uitvoeren op wereldwijde schaal en onvoorspelbare gebruikspatronen kunnen vertonen.  Flexibiliteit bij het schalen van het gegevensarchief is essentieel tijdens de toepassingslaag schalen zodat deze overeenkomen met de gebruiksvereisten.  U kunt schalen uit door extra gegevenspartities onder een DocumentDB-account toe te voegen.  Bovendien kunt u ook extra DocumentDB accounts maken tussen de verschillende regio's. Zie voor de DocumentDB regio servicebeschikbaarheid, [Azure regio's](https://azure.microsoft.com/regions/#services).   

## <a name="catalog-data"></a>Gegevens in de catalogus
Gebruiksscenario's voor catalogus gegevens betrekking hebben op te slaan en een set kenmerken voor entiteiten zoals mensen, plaatsen en producten zoeken.  Enkele voorbeelden van de gegevens in de catalogus zijn gebruikersaccounts, productcatalogi, apparaat-registers voor IoT en bill materialen systemen.  Kenmerken voor deze gegevens kunnen variëren en na verloop van tijd om te voldoen aan de toepassingsvereisten kunnen wijzigen.  

Bekijk een voorbeeld van een productcatalogus voor een leverancier van auto-onderdelen. Elk onderdeel kan zijn eigen kenmerken, naast de algemene kenmerken die alle onderdelen delen hebben.  Kenmerken voor een bepaald deel kunnen bovendien het volgende jaar, wanneer een nieuw model wordt vrijgegeven wijzigen.  Als een winkel JSON-document DocumentDB ondersteunt flexibele schema's en kunt u gegevens met geneste eigenschappen vertegenwoordigen, en het is dus heel geschikt voor het opslaan van gegevens in de productcatalogus.       

## <a name="logging-and-time-series-data"></a>Logboekregistratie en Time-series data
Toepassingslogboeken vaak in grote hoeveelheden wordt gegenereerd en kunnen hebben verschillende kenmerken op basis van de gedistribueerde toepassingsversie of de component registratie van gebeurtenissen.  Logboekgegevens is niet beperkt tot complexe relaties of starre structuren. Logboekgegevens worden steeds vaker behouden in JSON-indeling omdat JSON is licht en gemakkelijk voor de mens om te lezen.
   
Meestal zijn er twee belangrijke use-cases die betrekking hebben op gegevens uit gebeurtenislogboek.  De eerste use-case is het ad-hoc query's uitvoeren op een subset van gegevens voor het oplossen van problemen.  Tijdens het oplossen van problemen wordt een subset van gegevens eerst opgehaald uit de logboeken, meestal door tijdreeksen.  Vervolgens wordt een drilldownbewerking uitgevoerd door het filteren van de dataset met foutniveaus of foutberichten worden weergegeven. Dit is is waar de gebeurtenislogboeken op te slaan in de DocumentDB een voordeel. Logboekgegevens zijn opgeslagen in de DocumentDB wordt automatisch geïndexeerd standaard en het is dus nu op elk gewenst moment worden opgevraagd. Logboekgegevens kunt bovendien meerdere gegevenspartities worden vastgelegd als een tijdreeks. Oudere logboeken kunnen worden uitgerold voor gekoelde opslag per het bewaarbeleid.          

De tweede use-case bestaat uit langdurige data analytics taken off line worden uitgevoerd via een grote hoeveelheid logboekgegevens.  Voorbeelden van deze use-case zijn server beschikbaarheid analyse, analyse van toepassing fout en clickstream data-analyse.  Hadoop wordt meestal gebruikt voor het uitvoeren van deze analyses.  Met de Hadoop-Connector voor DocumentDB werken DocumentDB databases als gegevensbronnen en PUT voor taken varken, het component en kaart/verminderen. Zie voor details op de verbindingslijn Hadoop voor DocumentDB [uitvoeren van een taak Hadoop met DocumentDB en HDInsight](documentdb-run-hadoop-with-hdinsight.md).      

## <a name="gaming"></a>Games
De databaselaag is een belangrijk onderdeel van games. Moderne spellen grafische bewerkingen uitvoeren op clients mobile-console, maar zijn afhankelijk van de wolk voor het leveren van aangepaste en gepersonaliseerde inhoud zoals in het spel stats, sociale media integratie en scoreborden hoge score. Spelletjes moet zeer lage vertragingstijden voor leesbewerkingen en schrijfbewerkingen naar bieden een uitoefenen in het spel ervaring en de databaselaag nodig heeft voor het verwerken van hoge en lage waarden in tarieven aanvraag tijdens een nieuw spel gestart en functie-updates.

DocumentDB wordt gebruikt door de enorme schaal spellen zoals [de dood lopen: Land geen Man van](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) door de [Volgende Games](http://www.nextgames.com/), en [Halo 5: verzorgers](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Gebruik in beide gevallen, de belangrijkste voordelen van DocumentDB zijn de volgende:

- DocumentDB kunnen de prestaties worden geschaald naar boven of beneden elastically. Hierdoor kunnen spellen bijwerken profiel en statistieken van tientallen miljoenen gamers tegelijk verwerken door middel van een enkele API-aanroep.
- DocumentDB ondersteunt milliseconde leest en schrijft naar alle lag voorkomen tijdens het spel.
- Automatische indexering van DocumentDB kunt u filteren op meerdere verschillende eigenschappen in real-time, bijvoorbeeld Zoek spelers door hun interne speler-id's, of hun GameCenter, Facebook, Google-id's of query op basis van lidmaatschap van de speler in een guild. Dit is mogelijk zonder complexe indexeren of sharding infrastructuur te bouwen.
- Sociale voorzieningen, waaronder in-game chat-berichten, player guild lidmaatschappen uitdagingen voltooid, hoge score scoreborden en sociale grafieken zijn gemakkelijker te implementeren met een flexibele schema.
- DocumentDB als een beheerde platform-as-a-service (PaaS) vereist minimale installatie en beheer voor snelle iteratie werken en sneller op de markt.


## <a name="user-preferences-data"></a>Voorkeuren voor gebruikersgegevens
Tegenwoordig komen de meeste moderne web en mobiele toepassingen met complexe opvattingen en ervaringen. Deze opvattingen en ervaringen zijn meestal dynamisch, catering gebruikersvoorkeuren of stemming en huisstijl behoeften.  Toepassingen moeten daarom kunnen persoonlijke instellingen daadwerkelijk ophalen om gebruikersinterface-elementen en ervaringen sneller gerenderd. 

JSON is een effectieve UI-outgegevens vertegenwoordigen, zoals het is niet alleen licht, maar ook kan worden eenvoudig geïnterpreteerd door JavaScript.  DocumentDB biedt instelbare consistentie niveaus waardoor snelle leesbewerkingen met lage latentie schrijven. Opslaan van gegevens van UI lay-out met inbegrip van persoonlijke instellingen als JSON-documenten in DocumentDB is daarom een doeltreffend middel om deze gegevens via de verbinding.

## <a name="iot-and-device-sensor-data"></a>Sensorgegevens IoT en apparaat
Use-cases IoT delen vaak bepaalde patronen in hoe zij nemen, verwerken en opslaan van gegevens.  Deze systemen kunnen eerst gegevens inname die bursts van gegevens van sensoren apparaat van verschillende landinstellingen kunt nemen.  Deze systemen worden vervolgens verwerkt en streaming gegevens analyseren om te ontlenen real-time inzicht. En laatst maar niet in het minst, de meeste zo niet alle gegevens zal uiteindelijk terechtkomen in een gegevensarchief voor ad hoc query- en analytics.    

Microsoft Azure biedt uitgebreide services die kunnen worden gebruikt voor IoT use-cases.  Azure IoT-services zijn een reeks services waaronder Azure gebeurtenis Hubs, Azure DocumentDB Azure Stream Analytics, Azure melding Hub, Azure Machine Learning, Azure, HDInsight en PowerBI. 

Bursts van gegevens mag worden ingenomen door Azure gebeurtenis Hubs, zoals ingestie van hoge doorvoer van gegevens met lage latentie biedt. Ingenomen gegevens die moet worden verwerkt voor real-time inzicht kan worden softwareproducten op Azure Stream Analytics voor real-time analytics. Gegevens kan in de DocumentDB voor ad hoc query's worden geladen. Zodra de gegevens in de DocumentDB wordt geladen, kan de gegevens moeten worden opgevraagd.  De gegevens in de DocumentDB kan worden gebruikt als verwijzing naar gegevens als onderdeel van de real time analytics. Bovendien kunnen gegevens verder worden verfijnd en gegevenskoppeling DocumentDB met HDInsight voor varkens, component of kaart/verminderen taken verwerkt.  Geraffineerde gegevens geladen vervolgens terug naar de DocumentDB voor het melden.   

Zie voor een IoT monsteroplossing met DocumentDB, EventHubs en Storm, de [opslagplaats op GitHub hdinsight-storm-voorbeelden](https://github.com/hdinsight/hdinsight-storm-examples/).

Zie voor meer informatie over Azure aanbiedingen voor IoT, [maken het Internet van uw zaken](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx).

## <a name="next-steps"></a>Volgende stappen
 
U kunt een [account](https://azure.microsoft.com/pricing/free-trial/) maken en volg onze [pad leren](https://azure.microsoft.com/documentation/learning-paths/documentdb/) om meer informatie over DocumentDB en de informatie die u nodig hebt om te beginnen met DocumentDB. 

Of als u meer informatie wilt over klanten met behulp van DocumentDB, de volgende artikelen van de klant zijn beschikbaar:

- De [volgende spellen](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). De dode wandel: spel Man van grond soars tot #1 ondersteund door DocumentDB Azure.
- [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Hoe Halo 5 sociale gameplay met Azure DocumentDB geïmplementeerd.
- [Galerie met Cortana Analytics](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics galerie - een schaalbare community-site die is gebaseerd op Azure DocumentDB.
- [Koud kunstje](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Integrator toonaangevende geeft multinationale ondernemingen globaal inzicht in minuten met flexibele Cloud-technologieën.
- [Republiek nieuws](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Bedrijfsinformatie toevoegen aan het nieuws informatie te verstrekken met het doel burgers ingeschakeld. 
- [SGS International](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Voor een consistente kleur over de hele wereld Ga belangrijke merken naar SGS. En SGS verandert in Azure.
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Toonaangevend Telenor maakt gebruik van de cloud te verplaatsen met de snelheid van een opstarten. 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). De winkel van de toekomst wordt uitgevoerd op Snel zoeken en de eenvoudige gegevensoverdracht.
