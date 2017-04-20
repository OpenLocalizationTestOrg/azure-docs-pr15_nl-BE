<properties 
    pageTitle="DocumentDB Database vragen - Veelgestelde vragen | Microsoft Azure" 
    description="Antwoorden vinden op veelgestelde vragen over Azure DocumentDB een NoSQL document databaseservice voor JSON. Database vragen beantwoorden over capaciteit, prestaties en schaalbaarheid." 
    keywords="Database, veelgestelde vragen, documentdb, azure, Microsoft azure vragen"
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
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>Veelgestelde vragen over DocumentDB

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Database vragen over Microsoft Azure DocumentDB fundamentals

### <a name="what-is-microsoft-azure-documentdb"></a>Wat is Microsoft Azure DocumentDB? 
DocumentDB voor Microsoft Azure is een planeet schaal NoSQL document database-as-a-service die biedt uitgebreide query gegevens zonder schema's, helpt een configureerbare en betrouwbare prestaties, en snelle ontwikkeling, alles via een beheerde platform ondersteund door de energie- en bereiken van Microsoft Azure van razendsnelle snel. DocumentDB is de juiste oplossing voor web, mobiele, gaming en IoT toepassingen wanneer voorspelbare doorvoer, hoge beschikbaarheid, lage latentie en een schema vrije gegevensmodel zijn belangrijke vereisten. DocumentDB biedt flexibiliteit schema en rijke indexeren via een native JSON-gegevensmodel en transactionele ondersteuning voor meerdere documenten met geïntegreerde JavaScript bevat.  
  
Zie de [DocumentDB documentatiepagina](https://azure.microsoft.com/documentation/services/documentdb/)voor meer database vragen, antwoorden en instructies voor de implementatie en het gebruik van deze service.

### <a name="what-kind-of-database-is-documentdb"></a>Wat voor soort database is DocumentDB?
DocumentDB is een NoSQL document georiënteerd database waarin gegevens worden opgeslagen in JSON-indeling.  DocumentDB ondersteunt geneste, contained gegevens structuren die kunnen worden opgevraagd via een rijke DocumentDB [grammatica voor SQL-query](documentdb-sql-query.md). DocumentDB biedt hoge prestaties transactionele verwerking van server-side JavaScript via [opgeslagen procedures, triggers en door gebruiker gedefinieerde functies](documentdb-programming.md). De database biedt ook ondersteuning voor ontwikkelaars consistentie instelbare niveaus met bijbehorende [prestaties](documentdb-performance-levels.md).
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>DocumentDB databases heb tabellen als een relationele database (RDBMS)
Nee, DocumentDB gegevens worden opgeslagen in collecties van JSON-documenten.  Zie voor informatie over het DocumentDB, [model met DocumentDB en concepten](documentdb-resources.md). Zie voor meer informatie over hoe NoSQL oplossingen zoals DocumentDB van relationele oplossingen verschillen [NoSQL versus SQL](documentdb-nosql-vs-sql.md).

### <a name="do-documentdb-databases-support-schema-free-data"></a>DocumentDB-databases bieden ondersteuning voor schema-gegevens?
Ja, kan DocumentDB toepassingen voor het opslaan van willekeurige JSON-documenten zonder schema definition of tips. Gegevens zijn direct beschikbaar voor de query via de interface DocumentDB SQL-query.   

### <a name="does-documentdb-support-acid-transactions"></a>Ondersteunt DocumentDB zuur transacties?
Ja, ondersteunt DocumentDB cross-document transacties uitgedrukt als JavaScript opgeslagen procedures en triggers. Transacties worden binnen het bereik van een enkele partitie binnen elke collectie en uitgevoerd met zuur semantiek als alle of niets geïsoleerd van andere gelijktijdig worden uitgevoerd en de gebruikersnaam aanvragen.  Als er uitzonderingen worden veroorzaakt door de uitvoering van de server side JavaScript-code van toepassing, de hele transactie teruggedraaid. Zie voor meer informatie over transacties [databasetransacties programma](documentdb-programming.md#database-program-transactions).

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>Wat zijn de typische use-cases voor DocumentDB?  
DocumentDB is een goede keuze voor nieuwe web, mobiel, games en IoT toepassingen waar Automatische schaal, voorspelbare prestaties, volgorde van milliseconde responstijden en de mogelijkheid om query snel gegevens zonder schema's is belangrijk. DocumentDB geschikt voor de snelle ontwikkeling en ondersteuning van de voortdurende herhaling van application data modellen. Toepassingen die de gegenereerde gebruikersinhoud en gegevens beheren zijn [algemene use-cases voor DocumentDB](documentdb-use-cases.md).  

### <a name="how-does-documentdb-offer-predictable-performance"></a>Hoe DocumentDB de mogelijkheid biedt voorspelbare prestaties?
Een [aanvraag eenheid](documentdb-request-units.md) is de maatregel van doorvoer in DocumentDB. 1 RU komt overeen met de doorvoer van het ophalen van een document van 1KB. Elke bewerking in DocumentDB, waaronder lezen, schrijven, SQL-query's en uitvoeringen van de opgeslagen procedure heeft een deterministische RU op basis van de doorvoer die nodig zijn om de bewerking te voltooien. U kunt zien in plaats van na te denken over de CPU, i/o en geheugen en elke invloed van de doorvoer van toepassingsgegevens in een enkele RU-eenheid.

Elke collectie DocumentDB kan worden gereserveerd met ingerichte doorvoer in RUs doorvoer per seconde. U kunt afzonderlijke aanvragen voor het meten van hun waarden RU en collecties van de voorziening voor het verwerken van de totale som van de eenheden van de aanvraag voor alle aanvragen benchmark voor toepassingen van de schaal. U kunt ook schalen of verminder de doorvoer van de collectie als de behoeften van uw toepassing evolve. Voor meer informatie over eenheden van de aanvraag en voor hulp moet om uw verzameling te bepalen, Lees [beheren van prestaties en capaciteit](documentdb-manage.md) en probeer de [doorvoer Rekenmachine](https://www.documentdb.com/capacityplanner). 

### <a name="is-documentdb-hipaa-compliant"></a>Compatibel is met HIPAA-DocumentDB?
Ja, DocumentDB HIPAA-compatibel is. HIPAA vaststelt voor het gebruik, de openbaarmaking, en bescherming van de op afzonderlijk identificeerbare gezondheidsinformatie. Zie voor meer informatie, het [Vertrouwenscentrum van Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-documentdb"></a>Wat zijn de grenzen van de opslag van DocumentDB? 
Er is geen theoretische limiet voor de totale hoeveelheid gegevens die een collectie kunt opslaan in DocumentDB. Als u opslaan van gegevens binnen één collectie meer dan 250 GB wilt, kunt u [contact opnemen met ondersteuning](documentdb-increase-limits.md) voor uw account quota verhoogd hebben. 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>Wat zijn de grenzen van de doorvoer van DocumentDB? 
Er is geen theoretische limiet aan het totale bedrag van doorvoer die een collectie in DocumentDB ondersteunen kan, als uw werkbelasting kan worden verdeeld ongeveer gelijkmatig over een voldoende groot aantal partitiesleutels. Als u meer dan 250.000 aanvraag eenheden per seconde per collectie of de account wilt, kunt u [contact op met ondersteuning](documentdb-increase-limits.md) voor uw account quota verhoogd hebben. 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Hoeveel kost Microsoft Azure DocumentDB
Raadpleeg de pagina [DocumentDB prijsdetails](https://azure.microsoft.com/pricing/details/documentdb/) voor meer informatie. DocumentDB kosten worden bepaald door het aantal collecties in gebruik, het aantal uren dat de verzamelingen online zijn, en de opslag van verbruikte en ingerichte doorvoer voor elke collectie. 

### <a name="is-there-a-free-account-available"></a>Is er een gratis account beschikbaar?
Bent u nieuw in Azure, kunt u zich aanmelden voor een [gratis account Azure](https://azure.microsoft.com/free/), waarmee u 30 dagen en $200 om te proberen de Azure services. Of als u een Visual Studio-abonnement hebt, u in aanmerking komen voor [$150 in vrije Azure credits per maand](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) op een Azure service te gebruiken.  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>Hoe krijg ik meer hulp bij DocumentDB
Als u hulp nodig hebt, bereiken ons op [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-documentdb), de [Azure DocumentDB MSDN Developer Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)of plannen van een [1 op 1 chatten met het engineering-team van DocumentDB](http://www.askdocdb.com/). Blijf op de hoogte van het laatste nieuws van de DocumentDB en de functies, als u wilt ons te volgen op [Twitter](https://twitter.com/DocumentDB).

## <a name="set-up-microsoft-azure-documentdb"></a>Microsoft Azure DocumentDB instellen

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Hoe kan ik de aanmelding voor Microsoft Azure DocumentDB?
DocumentDB voor Microsoft Azure is beschikbaar in de [Portal Azure][azure-portal].  Eerst moet u aanmelden voor een abonnement op Microsoft Azure.  Zodra u zich voor een abonnement op Microsoft Azure aanmeldt, kunt u een DocumentDB-account toevoegen aan uw Azure-abonnement. Zie voor instructies over het toevoegen van een DocumentDB-account, [een account DocumentDB database maken](documentdb-create-account.md).   

### <a name="what-is-a-master-key"></a>Wat is een hoofdsleutel?
Een hoofdsleutel is een beveiligingstoken toegang krijgen tot alle bronnen voor een account. Personen met de sleutel hebben lees- en schrijftoegang tot alle bronnen in de database. Wees voorzichtig bij het distribueren van hoofdsleutels. De hoofdsleutel voor primaire en secundaire hoofdsleutel beschikbaar zijn in de **sleutels **blade van de [Azure Portal][azure-portal]. Zie voor meer informatie over sleutels [weergeven, kopiëren en opnieuw genereren toegangstoetsen](documentdb-manage-account.md#keys).

### <a name="how-do-i-create-a-database"></a>Hoe maak ik een database
U kunt databases met behulp van de [Portal Azure]() zoals beschreven in [een DocumentDB-database maken](documentdb-create-database.md), een van de [DocumentDB SDK's](documentdb-sdk-dotnet.md), of via de [REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx)maken.  

### <a name="what-is-a-collection"></a>Wat is een collectie?
Een collectie is een container van JSON-documenten en de bijbehorende JavaScript toepassingslogica. Een collectie is een entiteit te factureren, waarbij de [kosten](documentdb-performance-levels.md) wordt bepaald door de doorvoer- en storaged gebruikt. Verzamelingen kunnen een of meer partities/servers omvatten en is schaalbaar, zodat een vrijwel onbeperkt aantal volumes van opslag of doorvoer te verwerken.

Collecties zijn ook de facturering entiteiten voor de DocumentDB. Elke collectie wordt gefactureerd per uur op basis van de ingerichte doorvoer en de opslagruimte die wordt gebruikt. Zie voor meer informatie [DocumentDB prijzen](https://azure.microsoft.com/pricing/details/documentdb/).  

### <a name="how-do-i-set-up-users-and-permissions"></a>Hoe stel ik van gebruikers en machtigingen
U kunt gebruikers en machtigingen met behulp van een van de [DocumentDB SDK's](documentdb-sdk-dotnet.md) of via de [REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx)maken.   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Vragen over het ontwikkelen met Microsoft Azure DocumentDB database

### <a name="how-to-do-i-start-developing-against-documentdb"></a>Hoe wil ik start ontwikkelen tegen DocumentDB?
[SDK's](documentdb-sdk-dotnet.md) zijn beschikbaar voor .NET, Python, Node.js, JavaScript en Java.  Ontwikkelaars kunnen ook gebruikmaken van de [RESTful HTTP API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) om te communiceren met DocumentDB middelen uit verschillende platforms en talen. 

De monsters voor de DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md)en [Python](documentdb-python-samples.md) SDK's zijn beschikbaar op GitHub.

### <a name="does-documentdb-support-sql"></a>Ondersteunt DocumentDB SQL?
De DocumentDB SQL query-taal is een uitgebreide subset van de queryfunctionaliteit wordt ondersteund door SQL. De DocumentDB SQL query language biedt uitgebreide hiërarchische en relationele operators en uitbreidbaarheid via JavaScript op basis van gebruiker gedefinieerde functies (UDF's). JSON grammatica kan voor modellering van JSON-documenten als bomen met labels als de structuurknooppunten die wordt gebruikt door zowel de automatische indexering technieken voor DocumentDB, alsmede de query SQL-dialect van de DocumentDB.  Zie voor meer informatie over het gebruik van de SQL-grammatica, voer de [Query DocumentDB] [ query] artikel.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>Wat zijn de gegevenstypen die worden ondersteund door DocumentDB?
De primitieve gegevenstypen ondersteund in DocumentDB zijn hetzelfde als JSON. JSON is een eenvoudig typesysteem dat uit tekenreeksen, getallen (dubbele precisie IEEE754) en Booleans - true, false en null-waarden bestaat.  Meer complexe gegevenstypen zoals DateTime, Guid, Int64 en geometrie kunnen zowel in JSON en DocumentDB bij het maken van geneste objecten met behulp van de operator {} en matrices met de operator [] worden weergegeven. 

### <a name="how-does-documentdb-provide-concurrency"></a>Hoe biedt DocumentDB gelijktijdigheid?
DocumentDB ondersteunt OCC. (OCC) via HTTP eenheidtags of etags. Elke DocumentDB bevat een etag en de etag is ingesteld op de server telkens wanneer een document wordt bijgewerkt. De etag header en de huidige waarde worden in alle antwoordberichten opgenomen. Etags kan worden gebruikt met de header If-Match waardoor de server om te bepalen als een resource moet worden bijgewerkt. De If-Match-waarde is de waarde van de etag gecontroleerd. Als de etag-waarde overeenkomt met de waarde van de etag server, kunt u de bron wordt bijgewerkt. Als de etag niet meer actueel is, de server weigert de bewerking met een ' HTTP 412 voorwaarde mislukt "respons-code. De client moet vervolgens opnieuw ophalen van de resource voor de huidige etag-waarde voor de resource. Etags kan bovendien met de header If-None-Match worden gebruikt om te bepalen of een opnieuw ophalen van een resource nodig is. 

Als u optimistische gelijktijdigheid in .NET, gebruikt u de klasse [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) . Zie voor een voorbeeld .NET [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) in de steekproef DocumentManagement op github.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>Hoe voer ik transacties uit in DocumentDB?
DocumentDB ondersteunt transacties taal geïntegreerd via JavaScript opgeslagen procedures en triggers. Alle databasebewerkingen in scripts worden uitgevoerd binnen het bereik van de collectie als deze een verzameling van één partitie is, of met dezelfde partitie sleutelwaarde in een collectie documenten, als de collectie is gepartitioneerd overgeslagen. Een momentopname van de documentversies (ETags) is genomen bij het begin van de transactie en toegezegd alleen als het script is uitgevoerd. Als de JavaScript een fout genereert, de transactie teruggedraaid. Zie [DocumentDB server-side programmeren](documentdb-programming.md) voor meer informatie.

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>Hoe kan ik documenten invoegen in bulk in DocumentDB? 
Er zijn drie manieren om te bulk-documenten invoegen in DocumentDB:

- De data migration tool, zoals beschreven in de [DocumentDB gegevens worden geïmporteerd](documentdb-import-data.md).
- Document Explorer in Azure Portal, zoals beschreven in [grote hoeveelheden documenten met Document Explorer toevoegen](documentdb-view-json-document-explorer.md#BulkAdd).
- Opgeslagen procedures, zoals beschreven in de [DocumentDB server-side programmeren](documentdb-programming.md).

### <a name="does-documentdb-support-resource-link-caching"></a>DocumentDB ondersteunt resource koppeling caching?
Ja, omdat het DocumentDB is een RESTful service, resource koppelingen onveranderbaar zijn en in de cache kunnen worden opgeslagen. DocumentDB-clients kunnen u een header 'If-None-Match' opgeven voor leesbewerkingen tegen elke bron, zoals een document of collectie en update hun lokale opgehaald wanneer er op de server-versie heeft gewijzigd. 

### <a name="is-a-local-instance-of-documentdb-available"></a>Een lokaal exemplaar van DocumentDB beschikbaar is?
Een lokaal exemplaar van DocumentDB is op dit moment niet beschikbaar. U kunt de status van een lokale emulator en stem deze op het [Feedbackforum](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance)bijhouden.


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 
