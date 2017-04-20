<properties 
    pageTitle="Inleiding tot DocumentDB, een JSON-database | Microsoft Azure" 
    description="Informatie over Azure DocumentDB, een database NoSQL JSON. Dit document database is opgebouwd voor big data, elastische schaalbaarheid en beschikbaarheid." 
    keywords="JSON-database, document database"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/13/2016" 
    ms.author="mimig"/>

# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Inleiding tot de DocumentDB: een Database NoSQL JSON

##<a name="what-is-documentdb"></a>Wat is DocumentDB?

DocumentDB is een volledig beheerde NoSQL databaseservice gebouwd voor snelle en voorspelbare prestaties, hoge beschikbaarheid, elastische schalen, wereldwijde distributie en ontwikkeling te vereenvoudigen. Als een database NoSQL schema vrije biedt DocumentDB rijke en bekende SQL query-mogelijkheden met consistent lage vertragingstijden van JSON-gegevens - ervoor te zorgen dat 99% van uw leesbewerkingen bereikbaar zijn minder dan 10 milliseconden en 99% van uw geschreven onder de 15 milliseconden worden bediend. Deze unieke voordelen maken een geweldige DocumentDB geschikt voor web, mobiel, games, en IoT en vele andere toepassingen die naadloos schaal en replicatie van globale nodig.

## <a name="how-can-i-learn-about-documentdb"></a>Hoe kan ik meer informatie over DocumentDB? 

Een snelle manier om meer informatie over DocumentDB en de praktijk is deze drie stappen: 

1. De twee minuten kijken [Wat is DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) video, die de voordelen van het gebruik van DocumentDB geïntroduceerd.
2. Bekijk de drie minuten [DocumentDB op Azure maken](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) video's die worden gemarkeerd om te beginnen met DocumentDB via de Portal Azure.
3. Ga naar de [Speelplaats Query](http://www.documentdb.com/sql/demo), waar u verschillende activiteiten voor meer informatie over de uitgebreide query functionaliteit beschikbaar in DocumentDB kunt doorlopen. Vervolgens via het tabblad sandbox-head en uw eigen aangepaste SQL-query's uitvoeren en experimenteren met DocumentDB.

Vervolgens terug naar dit artikel, waar we je de afdruk.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Welke mogelijkheden en de belangrijkste functies DocumentDB biedt?  

Azure DocumentDB biedt de volgende belangrijke functies en voordelen:

-   **Elastically schaalbare doorvoer en opslag:** Eenvoudig vergroten of verkleinen van de DocumentDB JSON-database om te voldoen aan de behoeften van uw toepassing. Uw gegevens worden opgeslagen op solid state schijven (SSD) voor lage voorspelbare vertragingstijden. DocumentDB ondersteunt de containers voor de opslag van collecties die kunnen worden aangepast aan een vrijwel onbeperkte opslagruimte en ingerichte doorvoer genoemd JSON-gegevens. U kunt elastically schalen DocumentDB met voorspelbare prestaties naadloos groeien met uw toepassing. 

-   **-Replicatie met meerdere regio:** DocumentDB wordt transparant uw gegevens gerepliceerd naar alle regio's die u hebt gekoppeld aan uw account DocumentDB zodat u voor het ontwikkelen van toepassingen waarvoor de algemene toegang tot gegevens terwijl de wisselwerking tussen de consistentie, beschikbaarheid en prestaties, met de bijbehorende garanties. DocumentDB biedt transparante regionale failover met meerdere homing-API's en de mogelijkheid voor het schalen van doorvoer en opslag elastically over de hele wereld. Lees meer in [gegevens met DocumentDB wereldwijd te verspreiden](documentdb-distribute-data-globally.md).

-   **Ad hoc query's met een vertrouwde SQL-syntaxis:** Heterogene JSON-documenten binnen de DocumentDB opslaan en deze documenten via een vertrouwde SQL-syntaxis van een query. DocumentDB maakt gebruik van een gratis zeer gelijktijdige, vergrendeling, logboek gestructureerde indextechnologie automatisch alle om documentinhoud te indexeren. Hierdoor kunnen uitgebreide real-time query's zonder te hoeven geven tips schema, secundaire of weergaven. Meer informatie in de [Query DocumentDB](documentdb-sql-query.md). 

-   **JavaScript worden uitgevoerd in de database:** Opgeslagen procedures, triggers en door gebruiker gedefinieerde functies (UDF's) met JavaScript standaard druk toepassingslogica. Hierdoor kan uw toepassingslogica die gegevens gebruiken zonder u zorgen te maken over het verschil tussen de toepassing en het databaseschema. DocumentDB biedt volledige transactionele uitvoering van JavaScript toepassingslogica direct in de database-engine. De naadloze integratie van JavaScript kunt de uitvoering van het invoegen, vervangen, DELETE en SELECT bewerkingen uit in een JavaScript-programma als een afzonderlijke transactie. Meer informatie in [DocumentDB server-side programmeren](documentdb-programming.md).

-   **Consistentie instelbare niveaus:** Selecteer in vier niveaus van consistentie goed gedefinieerd voor een optimale verhouding tussen prestaties en de consistentie. Voor query's en leesbewerkingen DocumentDB biedt vier verschillende consistentie-niveaus: sterk, begrensd staleness sessie, en uiteindelijk. Deze gedetailleerde, goed-gedefinieerde consistentie niveaus kunnen u geluid e-mailverkeer tussen consistentie, beschikbaarheid en latentie. Meer informatie in [samenhang niveaus voor optimale beschikbaarheid en prestaties in de DocumentDB](documentdb-consistency-levels.md).

-   **Volledig beheerd:** Elimineren de noodzaak om database- en netwerkbronnen te beheren. Als een volledig beheerde Microsoft Azure service, u niet nodig hebt voor het beheren van virtuele machines, implementeren en configureren van software, schalen beheren, of omgaan met complexe gegevenslaag upgrades. Elke database wordt automatisch een back-up en beveiligd tegen storingen van regionale. U kunt eenvoudig een DocumentDB toevoegen en capaciteit inrichten als u nodig hebt, zodat u zich kunt concentreren op uw toepassing in plaats van het gebruik en beheer van uw database. 

-   **Open ontwerp:** Snel aan de slag met behulp van bestaande vaardigheden en hulpprogramma's. Programmering tegen DocumentDB is eenvoudig, toegankelijk en hoeft niet te voldoen aan de aangepaste extensies JSON of JavaScript of vaststellen van nieuwe hulpmiddelen. U kunt toegang krijgen tot alle databasefunctionaliteit van de met inbegrip van de CRUD-, query- en JavaScript-verwerking via een eenvoudige RESTful HTTP-interface. DocumentDB bestrijkt bestaande indelingen, talen en normen bij een hoge waarde bieden mogelijkheden op deze database.

-   **Automatische indexering:** Standaard DocumentDB [indexeert automatisch](documentdb-indexing.md) alle documenten in de database en niet verwachten of vereisen een schema of secundaire indexen te maken. Niet wilt indexeren alles? Maak je geen zorgen, kunt u [niet deelnemen aan de paden in JSON bestanden](documentdb-indexing-policies.md) ook.

##<a name="data-management"></a>Hoe DocumentDB gegevens beheren

Azure DocumentDB JSON-gegevens door middel van goed gedefinieerde database bronnen worden beheerd. Deze bronnen worden gerepliceerd voor een hoge beschikbaarheid en een unieke adresseerbaar door hun logische URI. DocumentDB biedt dat een eenvoudige HTTP gebaseerde RESTful programmeermodel voor alle resources. 

De account van de database DocumentDB is een unieke naamruimte waarmee u toegang krijgt tot Azure DocumentDB. Voordat u een databaseaccount maken kunt, moet u een abonnement Azure, waar u toegang tot tal van Azure services hebt hebben. 

Alle bronnen binnen de DocumentDB worden gemodelleerd en opgeslagen als JSON-documenten. Bronnen worden beheerd met metagegevens die zijn JSON-documenten als artikelen, en als feeds zijn verzamelingen artikelen. Sets van artikelen zijn opgenomen in hun respectieve feeds.

De onderstaande afbeelding ziet u de relaties tussen de DocumentDB middelen:

![De hiërarchische relatie tussen bronnen in DocumentDB, een database NoSQL JSON][1] 

Een databaseaccount bestaat uit een set van databases, elk met verschillende collecties, die elk opgeslagen procedures, triggers, UDF's, documenten en bijbehorende bijlagen kan bevatten. Een database horen ook gebruikers, elk met een set machtigingen voor toegang tot diverse andere collecties, opgeslagen procedures, triggers, UDF's, documenten of bijlagen. Terwijl de databases, gebruikers, machtigingen en collecties bronnen met bekende schema's het systeem gedefinieerde zijn-documenten, opgeslagen procedures, triggers, UDF's en bijlagen willekeurige bevatten, gedefinieerd door de gebruiker JSON-inhoud.  

##<a name="develop"></a>Hoe kan ik apps met DocumentDB ontwikkelen?

Azure DocumentDB beschrijft bronnen via een REST API die kan worden aangeroepen door een HTTP/HTTPS-verzoeken staat taal. Daarnaast biedt DocumentDB programming bibliotheken voor diverse populaire talen. Deze bibliotheken vereenvoudigen veel aspecten van het werken met Azure DocumentDB door de verwerking van gegevens zoals het adres in de cache opslaan, beheren van uitzonderingen, automatische pogingen, enzovoort. Bibliotheken zijn beschikbaar voor de volgende talen en platforms:  

Downloaden | Documentatie
--- | ---
[.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) | [.NET-bibliotheek](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) | [Bibliotheek node.js](http://azure.github.io/azure-documentdb-node/)
[Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) | [Java-bibliotheek](http://azure.github.io/azure-documentdb-java/)
[JavaScript-SDK](http://go.microsoft.com/fwlink/?LinkID=402991) | [JavaScript-bibliotheek](http://azure.github.io/azure-documentdb-js/)
n.v.t. | [Server-side JavaScript-SDK](http://azure.github.io/azure-documentdb-js-server/)
[Python-SDK](https://pypi.python.org/pypi/pydocumentdb) | [Python bibliotheek](http://azure.github.io/azure-documentdb-python/)

Basic buiten maken, lezen, bijwerken en verwijderen, DocumentDB biedt een uitgebreide interface voor SQL-query voor het ophalen van JSON-documenten en bestanden op de server ondersteuning voor transactionele uitvoering van toepassingslogica JavaScript. De uitvoering van query's en het script interfaces zijn beschikbaar via alle platform bibliotheken, alsmede de overige API's. 

### <a name="sql-query"></a>SQL-query
Azure DocumentDB documenten opvragen met behulp van een SQL-taal, die basis in het systeem van JavaScript en expressies met ondersteuning voor relationele, ruimtelijke en hiërarchische query's heeft zijn. De querytaal DocumentDB is een eenvoudige maar krachtige interface op query JSON-documenten. De taal ondersteunt een subset van de ANSI SQL-grammatica en naadloze integratie van JavaScript-object, matrices objectconstructie en functie-aanroep wordt toegevoegd. DocumentDB biedt de query model zonder een expliciet schema of indexing tips van de ontwikkelaar.

Gebruiker gedefinieerde functies (UDF's) kan worden geregistreerd bij de DocumentDB en waarnaar wordt verwezen als onderdeel van een SQL-query, waardoor de grammaticacontrole voor de ondersteuning van aangepaste toepassingslogica uitbreiden. Deze UDF's zijn geschreven als JavaScript-programma's en worden uitgevoerd binnen de database. 

DocumentDB biedt ook een LINQ query provider als onderdeel van de [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx)voor .NET-ontwikkelaars. 

### <a name="transactions-and-javascript-execution"></a>Transacties en de uitvoering van JavaScript
DocumentDB kunt u toepassingslogica schrijven als benoemde programma's die zijn geschreven in JavaScript volledig. Deze programma's zijn geregistreerd voor een verzameling en databasebewerkingen op de documenten binnen een bepaalde collectie kunnen uitgeven. JavaScript kan worden geregistreerd als een trigger, opgeslagen procedure of door gebruiker gedefinieerde functie wordt uitgevoerd. Triggers en opgeslagen procedures kunnen maken, lezen, bijwerken en verwijderen van documenten dat door gebruiker gedefinieerde functies als onderdeel van de logica van de query kan worden uitgevoerd zonder schrijftoegang tot de collectie uitvoeren.

JavaScript kan worden uitgevoerd binnen de DocumentDB is gemodelleerd na de begrippen die worden ondersteund door relationele databasesystemen, met JavaScript als een moderne vervanging voor Transact-SQL. Alle JavaScript-logica wordt uitgevoerd in een transactie van de ZURE met snapshot-isolatie. In de loop van de uitvoering als de JavaScript-een uitzondering genereert, vervolgens de gehele transactie wordt afgebroken.

## <a name="next-steps"></a>Volgende stappen
Hebt u al een account Azure? Vervolgens u kunt aan de slag met DocumentDB in de [Portal Azure](https://portal.azure.com/#gallery/Microsoft.DocumentDB) door het [maken van een account DocumentDB database](documentdb-create-account.md).

Geen Azure account? U kunt:

- Aanmelden voor een [gratis proefperiode Azure](https://azure.microsoft.com/free/), waarmee u 30 dagen en $200 om te proberen de Azure services. 
- Als u een MSDN-abonnement hebt, u in aanmerking komen voor [$150 in vrije Azure credits per maand](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) op een Azure service te gebruiken. 

Wanneer u klaar om meer te weten bent, Ga naar onze [pad leren](https://azure.microsoft.com/documentation/learning-paths/documentdb/) om te navigeren van alle trainingsmaterialen die beschikbaar zijn. 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 
