<properties 
    pageTitle="DocumentDB model met hiërarchische en concepten | Microsoft Azure" 
    description="Informatie over hiërarchische model van de DocumentDB van databases, collecties, door gebruiker gedefinieerde functie (UDF), documenten, machtigingen voor het beheren van bronnen en meer."
    keywords="Hiërarchische model, documentdb, azure, Microsoft azure"   
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-hierarchical-resource-model-and-concepts"></a>Hiërarchische model met DocumentDB en concepten

De database-entiteiten die worden beheerd met DocumentDB **middelen**genoemd. Elke resource wordt uniek aangeduid met een logische URI. U kunt communiceren met de resources met behulp van standaard HTTP-woorden, aanvraag en respons kopteksten en statuscodes. 

In dit artikel leest, kun je steeds de volgende vragen te beantwoorden:

- Wat is een model met de DocumentDB?
- Wat zijn resources en niet door gebruiker gedefinieerde resources gedefinieerd?
- Hoe adresseer ik een resource
- Hoe werk ik met collecties
- Hoe werk ik met opgeslagen procedures, triggers en door gebruiker gedefinieerde functies (UDF's)

## <a name="hierarchical-resource-model"></a>Hiërarchische model met
Zoals het volgende diagram wordt de hiërarchische DocumentDB- **model met** bestaat uit sets met bronnen onder een databaseaccount elke adresseerbare via een URI logische en stabiel. Een set van middelen zal worden verwezen als een **feed** in dit artikel. 

>[AZURE.NOTE] DocumentDB biedt een zeer efficiënte TCP-protocol wordt ook RESTful haar communicatiemodel via de [.NET client SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx).

![Hiërarchische model met DocumentDB][1]  
**Hiërarchische model met**   

Als u wilt werken met resources, moet u [een account DocumentDB database maken](documentdb-create-account.md) met behulp van uw abonnement op Azure. Een databaseaccount kan bestaan uit een set van **databases**, elk met verschillende **collecties**, die elk op hun beurt bevatten **opgeslagen procedures, triggers, UDF's, documenten** en bijbehorende **bijlagen** (functie). Een database horen ook **gebruikers**, elk met een set **machtigingen** voor toegang tot collecties, opgeslagen procedures, triggers, UDF's, documenten of bijlagen. Databases, gebruikers, machtigingen en collecties zijn resources systeem gedefinieerd met bekende schema's, documenten en bijlagen bevatten willekeurige, door de gebruiker gedefinieerde JSON-inhoud.  

|Resource   |Beschrijving
|-----------|-----------
|Database-account   |Een databaseaccount is gekoppeld aan een reeks verschillende databases en een vast bedrag van blob-opslag voor bijlagen (functie). U kunt een of meer database wijzigen met behulp van uw abonnement op Azure. Ga naar onze [prijzen pagina](https://azure.microsoft.com/pricing/details/documentdb/)voor meer informatie.
|Database   |Een database is een logische container van de opslag van documenten gepartitioneerd in collecties. Het is ook een gebruikerscontainer.
|Gebruiker   |De logische naamruimte voor het bereik van machtigingen. 
|Machtiging |Autorisatietoken die is gekoppeld aan een gebruiker voor een bepaalde resource.
|Collectie |Een collectie is een container van JSON-documenten en de bijbehorende JavaScript toepassingslogica. Een collectie is een entiteit te factureren, waarbij de [kosten](documentdb-performance-levels.md) worden bepaald door het prestatieniveau van de die is gekoppeld aan de collectie. Verzamelingen kunnen een of meer partities/servers omvatten en is schaalbaar, zodat een vrijwel onbeperkt aantal volumes van opslag of doorvoer te verwerken.
|Opgeslagen Procedure   |Toepassingslogica geschreven in JavaScript die is geregistreerd bij een collectie en transactioneel worden uitgevoerd in de database-engine.
|Trigger    |Geschreven in JavaScript uitgevoerd voor of na een invoeging toepassingslogica vervangen of verwijderen.
|UDF    |Toepassingslogica geschreven in JavaScript. UDF's kunnen u een aangepaste query-operator model en waardoor de kern van de querytaal DocumentDB uitbreiden.
|Document   |Door de gebruiker gedefinieerde (willekeurige) JSON-inhoud. Standaard geen schema dient te worden gedefinieerd of secundaire indices hoeft te worden verstrekt voor alle documenten die worden toegevoegd aan een collectie.
|(Voorbeeld) Bijlage   |Een bijlage is een speciaal document met verwijzingen en bijbehorende metagegevens voor externe blob/media. De ontwikkelaar kunt hebben de blob beheerd door DocumentDB of opslaan met een externe blob-provider zoals OneDrive, Dropbox, enz. 


## <a name="system-vs-user-defined-resources"></a>Systeem vs. de door de gebruiker gedefinieerd
Bronnen zoals database accounts, databases, collecties, gebruikers, machtigingen, opgeslagen procedures, triggers en UDF's - alle hebben een vast schema en systeembronnen worden genoemd. Daarentegen bronnen zoals documenten en bijlagen hebben geen beperkingen op het schema en zijn voorbeelden van de gebruiker zijn gedefinieerd. In DocumentDB, systeem- en door de gebruiker gedefinieerd resources worden weergegeven en beheerd als standaard-compatibele JSON. Alle resources, systeem of door de gebruiker gedefinieerd, zijn de volgende algemene eigenschappen.

> [AZURE.NOTE] Opmerking dat alle eigenschappen in een bron gegenereerd worden voorafgegaan door een onderstrepingsteken (_) in hun JSON representatie.

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Eigenschap</strong></p></td>
            <td valign="top"><p><strong>Gebruiker instelbare of door systeem gegenereerd?</strong></p></td>
            <td valign="top"><p><strong>Doel</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Door systeem gegenereerd</p></td>
            <td valign="top"><p>Door het systeem gegenereerde, unieke en hiërarchische id van de resource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Door systeem gegenereerd</p></td>
            <td valign="top"><p>ETag van de bron vereist voor OCC.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Door systeem gegenereerd</p></td>
            <td valign="top"><p>Laatste bijgewerkte tijdstempel van de bron</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Door systeem gegenereerd</p></td>
            <td valign="top"><p>Unieke adresseerbare URI van de bron</p></td>
        </tr>
        <tr>
            <td valign="top"><p>ID</p></td>
            <td valign="top"><p>Door systeem gegenereerd</p></td>
            <td valign="top"><p>De unieke naam van de resource (met dezelfde partitie sleutelwaarde) gedefinieerd door de gebruiker. Als de gebruiker geen id is opgegeven, een id wordt het systeem worden gegenereerd</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Wire-representatie van resources
DocumentDB schrijft eigen uitbreidingen niet aan de JSON-coderingen van standaard- of bijzondere; werkt met standaard-compatibele JSON-documenten.  
 
### <a name="addressing-a-resource"></a>Een resource-adressering
Alle bronnen zijn adresseerbare URI. De waarde van de eigenschap **_self** van een resource geeft de relatieve URI van de resource. De indeling van de URI bestaat uit de /\<-kanaal\>/ padsegmenten {_rid}:  

|Waarde van de _self |Beschrijving
|-------------------|-----------
|/DBS   |Diervoeders van databases onder de databaseaccount van een
|/DBS/ {dbName}  |Database met een id die overeenkomt met de waarde {dbName}
|/DBS/ {dbName} /colls/   |Diervoeders van collecties onder een database
|/DBS/ {dbName} /colls/ {collName} |De collectie met een id die overeenkomt met de waarde {collName}
|/DBS/ {dbName} /colls/ {collName} / docs    |Diervoeders van documenten in een collectie
|/DBS/ {dbName} /colls/ {collName} /docs/ {docId}    |Document met een id die overeenkomt met de waarde {doc}
|/users/ /DBS/ {dbName}   |Invoer van gebruikers bij een database
|/users/ {dbName} /DBS/ {gebruikersnaam}   |Gebruiker met een id die overeenkomt met de waarde {gebruiker}
|/users/ {dbName} /DBS/ {gebruikers-id} / machtigingen   |Diervoeders van machtigingen voor een gebruiker
|/DBS/ {dbName} /users/ {gebruikersnaam} /permissions/ {permissionId}    |Toestemming met een id die overeenkomt met de waarde {machtiging}
  
Elke resource heeft een unieke gebruiker gedefinieerde naam toegankelijk via de eigenschap id. Opmerking: voor documenten, als de gebruiker geen id geeft, onze ondersteunde SDK's automatisch genereert een unieke id voor het document. De id is van maximaal 256 tekens die uniek is binnen de context van een bepaalde bovenliggende resource string door de gebruiker gedefinieerd. 

Elke resource heeft ook een hiërarchische resource door systeem gegenereerd id (ook wel een RID genoemd), die beschikbaar is via de eigenschap _rid is. De RID codeert de gehele hiërarchie van een bepaalde bron en het is een handige interne weergave gebruikt voor het afdwingen van referentiële integriteit in een gedistribueerde wijze. Het RID is uniek binnen een databaseaccount en wordt alleen intern gebruikt door DocumentDB voor efficiënte routering zonder lookups cross-partitie. De waarden van de eigenschappen _rid en de _self zijn alternatieve zowel canonieke vorm van een resource. 

De DocumentDB REST API's ondersteuning voor resources adressering en routering van aanvragen door de id en de eigenschappen van _rid.

## <a name="database-accounts"></a>Database-accounts
U kunt een of meer DocumentDB database wijzigen met behulp van uw abonnement Azure inrichten.

U kunt [DocumentDB database rekeningen maken en beheren](documentdb-create-account.md) via de Azure-Portal op [http://portal.azure.com/](https://portal.azure.com/). Maken en beheren van een databaseaccount moet u toegangsrechten en kan alleen worden uitgevoerd onder uw abonnement Azure. 

### <a name="database-account-properties"></a>Database-eigenschappen
Als onderdeel van de inrichting en het beheer van de databaseaccount van een kunt u configureren en lezen van de volgende eigenschappen:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Naam van eigenschap</strong></p></td>
            <td valign="top"><p><strong>Beschrijving</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Beleid voor consistentie</p></td>
            <td valign="top"><p>Deze eigenschap instellen op het standaardniveau voor consistentie voor alle collecties onder uw databaseaccount configureren. U kunt het niveau van de samenhang op basis van per aanvraag met de kop [x-ms-consistentie-niveau] verzoek negeren. <p><p>Opmerking Deze eigenschap is alleen van toepassing op de <i>door de gebruiker gedefinieerde bronnen</i>. Alle systeem gedefinieerde bronnen zijn geconfigureerd voor ondersteuning leest/query's met een sterke samenhang.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Sleutels van de vergunning</p></td>
            <td valign="top"><p>Dit zijn de primaire en secundaire master als alleen-lezen sleutels die administratieve toegang tot alle bronnen onder de databaseaccount bieden.</p></td>
        </tr>
    </tbody>
</table>

Houd er rekening mee dat naast het inrichten, configureren en beheren van uw databaseaccount vanaf de Portal Azure, kunt u ook via programmering maken en DocumentDB database accounts beheren met behulp van de [API's Azure DocumentDB REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) als [client SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx).  

## <a name="databases"></a>Databases
Een database DocumentDB is een logische container van een of meer collecties en gebruikers, zoals in het volgende diagram wordt weergegeven. U kunt een onbeperkt aantal databases onder een account DocumentDB database beperkt aanbod.  

![Account en collecties hiërarchische databasemodel][2]  
**Een Database is een logische container van gebruikers en collecties**

Een database kan vrijwel onbeperkte documentopslag gepartitioneerd met collecties die de documenten in deze transactie domeinen bevatten. 

### <a name="elastic-scale-of-a-documentdb-database"></a>Elastische schaal van een database met DocumentDB
Een DocumentDB-database is standaard – variërend van enkele GB tot petabytes van SSD back documentopslag en doorvoer ingerichte elastisch. 

In tegenstelling tot een database in traditionele RDBMS, een database in DocumentDB is niet binnen het bereik van één computer. Met DocumentDB, zoals de schaal van uw toepassing nodig heeft om te groeien, kunt u meer collecties of databases. Inderdaad, diverse toepassingen in de eerste partij binnen Microsoft hebt gebruikt DocumentDB op een schaal van de consument zeer grote DocumentDB-databases elke met duizenden collecties met terabytes aan opslag van documenten maken. U kunt groeien of een database verkleinen door het toevoegen of verwijderen van collecties die voldoen aan de vereisten van de toepassing van de schaal. 

U kunt een willekeurig aantal collecties in een database aan de aanbieding maken. Elke collectie heeft back SSD opslag en doorvoer ingericht voor u, afhankelijk van de geselecteerde laag.

Een database van DocumentDB is ook een container van de gebruikers. Een gebruiker, in turn is een logische naamruimte voor een verzameling machtigingen waarmee toegesneden machtigingen en toegang tot collecties, documenten en bijlagen.  
 
Net als bij andere bronnen in het model met DocumentDB, databases kunnen worden gemaakt, vervangen, verwijderd, lezen of eenvoudig met [Azure DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) of met een van de [client-SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx)geïnventariseerd. DocumentDB garandeert een sterke samenhang lezen of de metagegevens van de bron van een database opvragen. Automatisch verwijderen van een database, zorgt u ervoor dat u geen toegang de collecties of gebruikers die zich daarin tot.   

## <a name="collections"></a>Collecties
Een collectie DocumentDB is een container voor uw JSON-documenten. Een collectie is ook een eenheid van de schaal voor transacties en query's. 

### <a name="elastic-ssd-backed-document-storage"></a>Back-elastische SSD opslag van documenten
Een collectie is intrinsiek elastisch - het automatisch vergroot en verkleind als u documenten toevoegen of verwijderen. Collecties zijn logische bronnen en kunnen een of meer fysieke partities of servers omvatten. Het aantal partities binnen een collectie wordt bepaald door de DocumentDB op basis van de omvang en de ingerichte doorvoer van de collectie. Elke partitie in DocumentDB heeft een vaste hoeveelheid SSD-backed opslag gekoppeld en wordt gerepliceerd voor hoge beschikbaarheid. Beheer partitie volledig wordt beheerd door DocumentDB Azure en er geen complexe code schrijven of beheren van uw partities. DocumentDB collecties zijn **vrijwel onbeperkte** opslag en doorvoer. 

### <a name="automatic-indexing-of-collections"></a>Automatische indexering van collecties
DocumentDB is een databasesysteem waar schema vrij. Zij niet uitgaan of vereisen een schema voor de JSON-documenten. Als u documenten aan een collectie toevoegt, DocumentDB automatisch geïndexeerd en zijn beschikbaar voor u om te vragen. Automatische indexering van documenten zonder schema of secundaire sleutels is een belangrijke mogelijkheid van DocumentDB en door schrijven geoptimaliseerd, lock vrij en gestructureerd logboek index onderhoud technieken is ingeschakeld. DocumentDB ondersteunt duurzame volume van zeer snelle schrijft terwijl nog steeds consistente query's. Document- en index opslag worden gebruikt voor het berekenen van de opslag die wordt gebruikt door elke collectie. U kunt de opslag- en e-mailverkeer gekoppeld indexeren door de indexering beleid configureert voor een collectie kunt beheren. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Het beleid van een collectie voor indexering configureren
Het indexeren beleid van elke collectie kunt u afweging prestaties en opslag die is gekoppeld aan de indexering. De volgende opties zijn beschikbaar als onderdeel van de indexing-configuratie:  

-   Kies of de collectie automatisch alle documenten of niet indexeert. Standaard worden alle documenten automatisch geïndexeerd. U kunt automatische indexering uitschakelen en selectief alleen specifieke documenten toevoegen aan de index. Omgekeerd kunt u selectief wilt uitsluiten van alleen specifieke documenten. U kunt dit bereiken door de automatische eigenschap in te stellen op true of false op de indexingPolicy van een collectie en het gebruik van de aanvraagheader [x-ms-indexingdirective] tijdens het invoegen, vervangen of verwijderen van een document.  
-   Kies of u wilt opnemen of uitsluiten van specifieke paden of patronen in uw documenten uit de index. Dit kunt u bereiken door de instelling van includedPaths en excludedPaths op de indexingPolicy van een collectie respectievelijk. U kunt ook de opslag- en e-mailverkeer voor het bereik en de hash-query's voor een specifiek pad patronen. 
-   (Overeenstemming) kiezen tussen synchrone en asynchrone (lazy) index-updates. De index wordt standaard synchroon bijgewerkt op elke invoegen, vervangen of verwijderen van een document aan de collectie. Hiermee kunt de query's ingaan op hetzelfde niveau als die van het document leest consistentie. Terwijl DocumentDB geoptimaliseerd schrijven is en aanhoudende volumes van document schrijft synchroon index onderhoud en consistente query's voor ondersteunt, kunt u bepaalde collecties lazily de index bijwerken. Lazy indexing verhoogt verder de schrijfprestaties en is ideaal voor bulk ingestie scenario's voor voornamelijk lezen zware collecties.

De indexing-beleid kan worden gewijzigd door het uitvoeren van een PUT in de collectie. Dit kan worden bereikt door de [client-SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)en de [Azure Portal](https://portal.azure.com) of de [Azure DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx).

### <a name="querying-a-collection"></a>Een collectie opvragen
De documenten binnen een verzameling willekeurige schema's kunnen hebben en u documenten binnen een collectie kunt opvragen zonder dat u een schema of secundaire indices op voorhand. U kunt de collectie met de [DocumentDB SQL-syntaxis](https://msdn.microsoft.com/library/azure/dn782250.aspx), waarmee RTF hiërarchische, relationele en ruimtelijke operators en uitbreidbaarheid via JavaScript gebaseerde UDF's opvragen. JSON grammatica kan voor modellering van JSON-documenten als bomen met labels als structuurknooppunten. Dit is zowel door de automatische indexering technieken van DocumentDB als DocumentDB van SQL-dialect misbruikt. De querytaal DocumentDB bestaat uit drie belangrijke aspecten:   

1.  Een kleine set querybewerkingen die natuurlijk toewijzen aan de boomstructuur met inbegrip van hiërarchische query's en projecties. 
2.  Een subset van relationele bewerkingen, met inbegrip van de samenstelling, filter, projecties, aggregaten en self joins. 
3.  Pure JavaScript gebaseerde UDF's die met werken (1) en (2).  

Het model van de query DocumentDB probeert een evenwicht tot stand tussen functionaliteit, efficiëntie en eenvoud. De database-engine DocumentDB zelf compileert en voert de query SQL-instructies. U kunt een collectie met de [Azure DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) of een van de [client-SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx)opvragen. De .NET SDK wordt geleverd met een LINQ provider.

> [AZURE.TIP] U kunt uitproberen DocumentDB en SQL-query's uitvoeren op onze dataset in de [Query Speelplaats](https://www.documentdb.com/sql/demo).

### <a name="multi-document-transactions"></a>Transacties met meerdere documenten
Databasetransacties bieden een veilige en voorspelbare programmeermodel voor het omgaan met gelijktijdige wijzigingen in de gegevens. In RDBMS is de traditionele manier om te schrijven business logica te schrijven van **opgeslagen procedures** en/of **triggers** en verzendt het naar de databaseserver voor transactionele uitvoering. In RDBMS, de programmeur toepassing moet omgaan met verschillende programmeertalen: 

- De (niet-transactionele) application programming language (bijvoorbeeld JavaScript, Python, C#, Java, enz.)
- T-SQL, transactionele programmeertaal die standaard is uitgevoerd door de database

Op grond van haar diepe gehechtheid aan JavaScript en JSON direct in de database-engine biedt DocumentDB een intuïtieve programmeermodel voor het uitvoeren van JavaScript op basis van toepassingslogica rechtstreeks op de collecties van opgeslagen procedures en triggers. Hierdoor kunnen beide van de volgende opties:

- Efficiënte implementatie van gelijktijdigheid regelen, het herstellen van een automatische indexering van de grafieken JSON-object rechtstreeks in de database-engine
- Uitdrukken van nature Controlestroom, variabele scoping, toewijzing en integratie van primitieven met databasetransacties rechtstreeks in de programmeertaal JavaScript afhandeling van uitzonderingen

De JavaScript-logica op een niveau geregistreerd kunt verlenen databasebewerkingen in de documenten van de opgegeven collectie. DocumentDB loopt impliciet de JavaScript op basis van opgeslagen procedures en triggers in een omgevingstemperatuur zuur transacties met de snapshot-isolatie tussen documenten binnen een collectie. In de loop van de uitvoering als de JavaScript-een uitzondering genereert, vervolgens de gehele transactie wordt afgebroken. Het resulterende programmeermodel is een zeer eenvoudige maar krachtige. JavaScript ontwikkelaars beschikken nu over een "duurzaam" programmeermodel terwijl u ook de bekende taalconstructies en bibliotheek primitieven.   

De mogelijkheid om JavaScript uit te voeren rechtstreeks in de database-engine in dezelfde adresruimte als de buffergroep kan zodat en transactionele uitvoering van de verrichtingen van de database ten opzichte van de documenten van een collectie. Bovendien DocumentDB-database-engine maakt een diepe gehechtheid aan de JSON en JavaScript elimineert een impedantie niet overeen met de systemen van het type van de toepassing en de database.   

Na het maken van een collectie, kunt u opgeslagen procedures, triggers en UDF's met een collectie met de [Azure DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) of een van de [client-SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx)registreren. Na registratie kunt u verwijzen naar en uitvoeren. Houd rekening met de volgende opgeslagen procedure volledig in JavaScript is geschreven, de volgende code twee argumenten (naam en naam van de auteur) en maakt een nieuw document, een query voor een document en vervolgens bijgewerkt – alle binnen een impliciete zuur transactie. Op elk gewenst moment tijdens de uitvoering als een JavaScript-uitzondering wordt gegenereerd, de gehele transactie wordt afgebroken.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

De client kan "schip" de bovenstaande JavaScript logica aan de database voor transactionele worden uitgevoerd via een HTTP POST. Zie voor meer informatie over het gebruik van HTTP-methoden [RESTful interacties met DocumentDB middelen](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


U ziet dat omdat de database native JSON en JavaScript begrijpt, er is geen niet-overeenkomende typen systeem, geen 'Toewijzing of' of code generatie magic nodig.   

Opgeslagen procedures en triggers werken met een verzameling en de documenten in een collectie door middel van een goed gedefinieerd objectmodel, waarmee de huidige context van de collectie.  

Collecties in de DocumentDB kunnen worden gemaakt, verwijderd, lezen of eenvoudig met behulp van de [Azure DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) of een van de [client-SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx)geïnventariseerd. DocumentDB biedt altijd sterke samenhang lezen of bij het controleren van de metagegevens van een collectie. Automatisch verwijderen van een collectie zorgt ervoor dat u geen toegang de documenten, bijlagen, opgeslagen procedures, triggers tot en UDF's daarin.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Opgeslagen procedures, triggers en door gebruiker gedefinieerde functies (UDF)
Zoals beschreven in de vorige sectie, kunt u toepassingslogica rechtstreeks vanuit een transactie in de database-engine. De toepassingslogica kan worden geschreven in JavaScript volledig en kan worden gemodelleerd als een opgeslagen procedure, trigger of een UDF. De JavaScript-code in een opgeslagen procedure of een trigger kunt invoegen, vervangen, verwijderen, lees- of documenten binnen een collectie. Aan de andere kant kan de JavaScript binnen een UDF invoegen, vervangen of documenten verwijderen. UDF's inventariseren van de documenten van de resultaatset van een query en een andere resultatenset produceren. DocumentDB afgedwongen voor meerdere pacht, een strikte reservering op basis van resource governance. Elke opgeslagen procedure, trigger of een UDF wordt een vaste quantum operating system resources zijn werk doen. Opgeslagen procedures, triggers of UDF's verder kunnen geen koppeling met externe JavaScript-bibliotheken en als ze groter zijn dan de aan hen toegewezen resource-budgetten zijn gebeurd. U kunt registreren, de registratie van opgeslagen procedures, triggers of UDF's met een collectie met behulp van de REST API's.  Bij de registratie van is een opgeslagen procedure, trigger of een UDF vooraf gecompileerd en opgeslagen als byte-code die later wordt uitgevoerd. In het volgende gedeelte ziet u hoe u kunt de SDK DocumentDB JavaScript te registreren, uitvoeren en de registratie van een opgeslagen procedure, trigger en een UDF. De JavaScript-SDK is een eenvoudige wrapper op de [DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

### <a name="registering-a-stored-procedure"></a>Een opgeslagen procedure registreren
Registratie van een opgeslagen procedure maakt u een nieuwe bron voor de opgeslagen procedure op een collectie via HTTP POST.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };
    
    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Uitvoeren van een opgeslagen procedure.
Uitvoering van een opgeslagen procedure wordt uitgevoerd door de afgifte van een HTTP POST ten opzichte van een bestaand middel voor de opgeslagen procedure met parameters doorgeven aan de procedure in het hoofdgedeelte van de aanvraag.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Afmelden van een opgeslagen procedure
Afmelden van een opgeslagen procedure wordt gewoon door het verwijderen van een HTTP-ten opzichte van een bestaand middel voor de opgeslagen procedure uitgevoerd.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Een trigger vooraf registreren
Registratie van een trigger wordt uitgevoerd door het maken van nieuwe trigger-bronnen op een collectie via HTTP POST. U kunt opgeven of de trigger is een pre een trigger boeken en het type bewerking gekoppeld (bijvoorbeeld maken, vervangen, verwijderen of alle zijn kan).   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }
    
    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Uitvoeren van een pre-trigger
Uitvoering van een trigger wordt uitgevoerd door de naam van een bestaande trigger opgeven op het moment van verzenden boeken/plaatsen/verwijderen van de documentbron van een via de verzoek-header.  
 
    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Afmelden van een pre-trigger
Afmelden van een trigger wordt gewoon gedaan via een HTTP-verwijderen ten opzichte van een bestaande trigger bron uitgeven.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Een UDF registreren
Registratie van een UDF wordt gedaan door het maken van een nieuwe UDF-bron op een collectie via HTTP POST.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>Een UDF als onderdeel van de query wordt uitgevoerd
Een UDF kan worden opgegeven als onderdeel van de SQL-query en wordt gebruikt als een manier om uit te breiden de kern [SQL query-taal van DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Afmelden van een UDF 
Afmelden van een UDF is gewoon gedaan door het verwijderen van een HTTP-ten opzichte van een bestaand middel voor UDF.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Hoewel de bovenstaande fragmenten de registratie (POST), verwijdering (PUT), lezen/list (GET) en kan worden uitgevoerd (POST) via de [DocumentDB JavaScript SDK](https://github.com/Azure/azure-documentdb-js), kunt u ook de [REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) of andere [client SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx)gebruiken. 

## <a name="documents"></a>Documenten
U kunt invoegen, vervangen, verwijderen, lezen, inventariseren en willekeurige JSON-documenten in een collectie opvragen. DocumentDB schrijft niet voor elk schema en hoeft geen secundaire ter ondersteuning bij het controleren van documenten in een collectie.   

Een echt open databaseservice wordt, DocumentDB wordt niet verzint eventuele speciale gegevenstypen (bv. datum tijd) of specifieke coderingen voor JSON-documenten. Houd er rekening mee dat DocumentDB niet een speciale JSON verdragen vereist tot codificatie van de relaties tussen de verschillende documenten; de SQL-syntaxis van DocumentDB biedt een zeer krachtige query voor hiërarchische en relationele operators in query- en documenten zonder speciale aantekeningen of noodzaak tot codificatie van relaties tussen documenten met verschillende eigenschappen.  
 
Als met alle andere bronnen, kunnen documenten worden gemaakt, vervangen, verwijderd, lezen, geïnventariseerd en opgevraagde gemakkelijk met REST API's of met een van de [client-SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx). De quota die overeenkomen met alle geneste bijlagen maakt onmiddellijk als u een document. Het niveau van meer consistentie van documenten volgt het beleid consistent op de rekening van de database. Dit beleid kan worden overschreven op basis van per aanvraag afhankelijk van de vereisten voor de consistentie van gegevens van uw toepassing. Bij het opvragen van documenten, volgt de consistentie lezen de indexing-modus ingesteld op de collectie. Voor 'consistent', dit komt overeen met het accountbeleid consistentie. 

## <a name="attachments-and-media"></a>Bijlagen en media
>[AZURE.NOTE] Bijlage en media bronnen zijn voorbeeld functies.
 
DocumentDB kunt u voor het opslaan van binaire BLOB's / media met DocumentDB of aan uw eigen externe media archief. Ook kunt u de metagegevens van een medium in een speciaal document met de naam van de bijlage vertegenwoordigt. Een bijlage in de DocumentDB is een speciaal document (JSON) die verwijst naar de media/blob die elders zijn opgeslagen. Een bijlage is gewoon een speciaal document waarmee de metagegevens van een medium dat is opgeslagen in een externe Mediaopslag (bv. locatie, auteur, enz.) worden vastgelegd. 

Kunt u een toepassing van sociale lezen die DocumentDB gebruikt voor het opslaan van aantekeningen in inkt en opmerkingen, met inbegrip van de metagegevens worden gemarkeerd, bladwijzers, beoordelingen, enz., die voor een e-book van een bepaalde gebruiker alsof/ervaringen.   

-   De inhoud van het boek zelf is opgeslagen in de Mediaopslag of als onderdeel van de DocumentDB database-account of een externe media winkel beschikbaar. 
-   Een toepassing kan de metagegevens van elke gebruiker opslaan als een afzonderlijk document, b.v. Joe's metagegevens voor book1 is opgeslagen in een document waarnaar wordt verwezen door /colls/joe/docs/book1. 
-   Bijlagen verwijst naar de inhoudspagina's van een bepaald boek van een gebruiker worden opgeslagen in het bijbehorende document bijvoorbeeld /colls/joe/docs/book1/chapter1, /colls/joe/docs/book1/chapter2 enz. 

Houd er rekening mee dat de bovenstaande voorbeelden beschrijvende ID's gebruiken om de hiërarchie met bron over te brengen. Bronnen zijn toegankelijk via de REST API's via unieke resource-id. 

Voor de media die wordt beheerd door DocumentDB, wordt de eigenschap _media van de bijlage verwijst naar de media door de URI. DocumentDB garandeert dat aan vuilnis verzamelen de media wanneer onverwerkte referenties worden weggehaald. DocumentDB automatisch genereert de bijlage bij het uploaden van de nieuwe media en de _media om te verwijzen naar de zojuist toegevoegde media wordt gevuld. Als u kiest voor het opslaan van de media in een externe blob-archief beheerd door u (bijvoorbeeld OneDrive, opslag, Azure, DropBox etc), kunt u bijlagen om te verwijzen naar de media. In dit geval u de bijlage zelf maken en vullen van de eigenschap _media.   

Net als bij alle andere bronnen bijlagen kunnen worden gemaakt, vervangen, verwijderd, lezen of eenvoudig met REST API's of met een van de client-SDK's geïnventariseerd. Als u met documenten, volgt het niveau meer consistentie van bijlagen het beleid consistent op de rekening van de database. Dit beleid kan worden overschreven op basis van per aanvraag afhankelijk van de vereisten voor de consistentie van gegevens van uw toepassing. Bij het opvragen van de bijlagen, volgt de consistentie lezen de indexing-modus ingesteld op de collectie. Voor 'consistent', dit komt overeen met het accountbeleid consistentie. 
 
## <a name="users"></a>Gebruikers
Een DocumentDB-gebruiker vertegenwoordigt een logische naamruimte voor het groeperen van machtigingen. DocumentDB van een gebruiker kan overeenstemmen met een gebruiker in een identity-management-systeem of een vooraf gedefinieerde toepassingsrol. Voor DocumentDB vertegenwoordigt een gebruiker alleen een abstractie om een set machtigingen in een database.   

Voor de toepassing van meerdere pacht in uw toepassing, kunt u gebruikers in DocumentDB die overeenkomt met de werkelijke gebruikers of de huurders van de toepassing. Vervolgens kunt u machtigingen voor een bepaalde gebruiker die met het access-besturingselement via verschillende collecties, documenten, bijlagen, enz overeenkomen.   

Als uw toepassingen hoeft te schalen met de groei van uw gebruikers, kunt u uw gegevens kunt op verschillende manieren shard vast. U kunt een model van elk van uw gebruikers als volgt:   

-   Elke gebruiker wordt toegewezen aan een database.
-   Elke gebruiker wordt toegewezen aan een collectie. 
-   Documenten die overeenstemmen met meerdere gebruikers gaat u naar een specifieke collectie. 
-   Documenten die overeenstemmen met meerdere gebruikers gaat u naar een aantal collecties.   

Ongeacht de specifieke sharding strategie die u kiest, kunt u de werkelijke gebruikers als gebruikers in de database DocumentDB model en probleemloos korrelig machtigingen aan elke gebruiker koppelen.  

![Gebruiker collecties][3]  
**Sharding strategieën en modellering gebruikers**

Net als alle andere bronnen gebruikers in DocumentDB kunnen worden gemaakt, vervangen, verwijderd, lezen of eenvoudig met REST API's of met een van de client-SDK's geïnventariseerd. DocumentDB biedt altijd sterke samenhang lezen of bij het controleren van de metagegevens van een gebruiker een resource. Het is ongepast dat een gebruiker automatisch verwijderen zorgt ervoor dat u geen toegang de machtigingen van deze tot. Hoewel de DocumentDB haalt het quotum van de machtigingen als onderdeel van de verwijderde gebruiker op de achtergrond, de verwijderde machtigingen weer beschikbaar is direct te gebruiken.  

## <a name="permissions"></a>Machtigingen
Vanuit een access control perspectief, bronnen, zoals accounts, database, databases, gebruikers en machtigingen worden beschouwd als *administratieve* bronnen omdat deze machtigingen vereist. Aan de andere kant zijn bronnen, met inbegrip van de collecties, documenten, bijlagen, opgeslagen procedures, triggers en UDF's binnen het bereik onder een bepaalde database en *toepassingsbronnen*beschouwd. Overeenkomt met de twee typen bronnen en de rollen die u toegang deze (namelijk de beheerder- en tot), het model van de vergunning definieert twee soorten *toegangstoetsen*: *master* en *resource-sleutel*. De hoofdsleutel is een onderdeel van de databaseaccount en afkomstig zijn van de ontwikkelaar (of beheerder) die de databaseaccount is ingericht. Deze hoofdsleutel is beheerder semantiek, dat kan worden gebruikt om de toegang tot bronnen van administratieve en toepassing. De sleutel voor een resource is daarentegen een granulaire sneltoets waarmee u toegang krijgt tot een *bepaalde* bron van toepassing. Zo wordt de relatie tussen de gebruiker van een database en de machtigingen van de gebruiker voor een bepaalde resource (bijvoorbeeld collectie, document, bijlagen, opgeslagen procedure, trigger of UDF) vastgelegd.   

De enige manier om de sleutel van een resource wordt door het maken van een bron machtiging onder een bepaalde gebruiker. Houd er rekening mee dat het maken of ophalen van een machtiging, een hoofdsleutel moet worden gepresenteerd in de koptekst van de vergunning. Een machtiging resource koppelt het middel, de toegang en de gebruiker. Na het maken van een machtiging resource, moet de gebruiker alleen om de bijbehorende resource-toets voor toegang tot de gewenste resource. Daarom kan een sleutel voor de resource worden weergegeven als een logische en compacte weergave van de bron van de machtiging.  

Net als bij alle andere bronnen, machtigingen in DocumentDB kunnen worden gemaakt, vervangen, verwijderd, lezen of eenvoudig met REST API's of met een van de client-SDK's geïnventariseerd. DocumentDB biedt altijd sterke samenhang lezen of bij het controleren van de metagegevens van een machtiging. 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het werken met resources via HTTP-opdrachten in [RESTful interacties met DocumentDB middelen](https://msdn.microsoft.com/library/azure/mt622086.aspx).


[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

