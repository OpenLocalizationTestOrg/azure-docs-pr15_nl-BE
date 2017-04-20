<properties 
    pageTitle="DocumentDB programmeren: opgeslagen procedures, databasetriggers en UDF's | Microsoft Azure" 
    description="Informatie over het gebruik van DocumentDB voor het schrijven van opgeslagen procedures en databasetriggers door gebruiker gedefinieerde functies (UDF's) in JavaScript. Gebruik de database programing tips en meer." 
    keywords="Databasetriggers, opgeslagen procedure, opgeslagen procedure, database-programma, sproc, documentdb, azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>DocumentDB server-side programmeren: opgeslagen procedures, databasetriggers en UDF's

Informatie over hoe de taal van Azure DocumentDB geïntegreerd, transactionele uitvoering van JavaScript kan ontwikkelaars van **opgeslagen procedures**, **triggers** en **door de gebruiker gedefinieerde functies (UDF's)** worden geschreven in JavaScript. Hiermee kunt u een database programma toepassingslogica die kan worden verzonden en uitgevoerd op de database opslag partities rechtstreeks schrijven 

Het is raadzaam om aan de slag door de volgende video, waar Andrew Liu een korte inleiding tot de DocumentDB van server-side programming databasemodel biedt bekijken. 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

Ga vervolgens terug naar dit artikel, waar u leert hoe u de antwoorden op de volgende vragen:  

- Hoe schrijf ik een een opgeslagen procedure, trigger of UDF met JavaScript?
- Hoe garandeert DocumentDB zuur
- Hoe werken transacties in DocumentDB?
- Wat zijn vooraf activeert na de gebeurtenis en hoe schrijf ik een?
- Hoe ik registreren en uitvoeren van een opgeslagen procedure, trigger of UDF RESTful wijze met behulp van HTTP?
- Wat DocumentDB SDK's zijn beschikbaar voor het maken en uitvoeren van opgeslagen procedures, triggers en UDF's?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Inleiding tot de opgeslagen Procedure en de UDF-programmering

Deze aanpak van *"JavaScript als moderne dag T-SQL"* vrij, ontwikkelaars van de toepassing van de complexiteit van het systeem niet-overeenkomende typen en object-relationele gegevens technologieën. Het heeft ook een aantal intrinsieke voordelen die kunnen worden gebruikt om krachtige toepassingen te bouwen:  

-   **Procedurele logica:** JavaScript als een hoog niveau programmeertaal, biedt een rijke en bekende interface Express bedrijfslogica. U kunt complexe sequenties van dichter bewerkingen op de gegevens uitvoeren.

-   **Atomische transacties:** DocumentDB zorgt ervoor dat databasebewerkingen in één opgeslagen procedure uitvoeren of trigger atomaire zijn. Hiermee kunt een toepassing gerelateerde bewerkingen in één batch combineren zodat ze allemaal slagen of geen van hen slagen. 

-   **Prestaties:** Het feit dat JSON intrinsiek is toegewezen aan de taal Javascript systeem en ook de basiseenheid voor opslag in DocumentDB is kan een aantal optimalisaties zoals lazy intreding van JSON-documenten in de buffergroep en waardoor ze op verzoek beschikbaar voor het uitvoeren van code. Er zijn meer voordelen die zijn gekoppeld aan verzendkosten bedrijfslogica in de database:
    -   Batchverwerking: ontwikkelaars kunnen bewerkingen, zoals inserts groeperen en deze in bulk verzenden. De latentie van het netwerkverkeer kosten en de overhead van het archief voor het maken van afzonderlijke transacties aanzienlijk verminderd. 
    -   Vóór compilatie: DocumentDB precompiles, opgeslagen procedures, triggers en door gebruiker gedefinieerde functies (UDF's) om te voorkomen dat JavaScript compilatie kosten voor elke aanroep. De overhead van het bouwen van de byte-code voor de procedurele logica is op een minimale waarde afgelost.
    -   Toekenning van reeksnummers – veel bewerkingen moet een side-effect ("trigger") die mogelijk het uitvoeren van één of meerdere secundaire store operations. Afgezien van atomiciteit is dit meer zodat wanneer verplaatst naar de server. 
-   **Inkapseling:** Opgeslagen procedures kunnen worden gebruikt om bedrijfslogica op één plaats. Dit heeft twee voordelen:
    -   Een abstractie laag boven op de onbewerkte gegevens waarmee gegevens architecten evolueren hun toepassingen onafhankelijk van de gegevens worden toegevoegd. Dit is bijzonder voordelige wanneer de gegevens minder schema, vanwege de brosse veronderstellingen die moeten omgaan met de gegevens rechtstreeks in de toepassing worden baked mogelijk moet zijn.  
    -   Deze onttrekking kan bedrijven hun gegevens beveiligen door het stroomlijnen van de toegang van de scripts.  

Het maken en uitvoeren van de databasetriggers, opgeslagen procedure en aangepaste query's wordt ondersteund door de [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx), [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)en [client SDK's](documentdb-sdk-dotnet.md) in verschillende platformen inclusief .NET, Node.js en JavaScript.

**Dat in deze zelfstudie wordt [Node.js SDK met Q](http://azure.github.io/azure-documentdb-node-q/) van de** syntaxis en het gebruik van opgeslagen procedures, triggers en UDF's illustreren.   

## <a name="stored-procedures"></a>Opgeslagen procedures

### <a name="example-write-a-simple-stored-procedure"></a>Voorbeeld: Een eenvoudige opgeslagen procedure schrijven 
Laten we beginnen met een eenvoudige opgeslagen procedure die een "Hello World"-antwoord als resultaat geeft.

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


Opgeslagen procedures per collectie worden geregistreerd en kunnen worden toegepast op een document en de bijlage in die collectie aanwezig. In het volgende fragment laat zien hoe de helloWorld opgeslagen procedure met een verzameling te registreren. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Als de opgeslagen procedure is geregistreerd, kunnen we uitgevoerd op de collectie en de resultaten terug op de client lezen. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


De contextobject biedt toegang tot alle bewerkingen die kunnen worden uitgevoerd op DocumentDB opslag en toegang tot de objecten van de aanvraag en het antwoord. In dit geval we het response-object gebruikt voor het instellen van de hoofdtekst van het antwoord dat is verzonden naar de client. Raadpleeg de [DocumentDB JavaScript server SDK-documentatie](http://azure.github.io/azure-documentdb-js-server/)voor meer informatie.  

Laat ons op dit voorbeeld uitbreiden en voeg dat meer database gerelateerde functionaliteit aan de opgeslagen procedure. Opgeslagen procedures kunnen maken, bijwerken, lezen, zoeken en verwijderen van documenten en bijlagen in de collectie.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Voorbeeld: Een opgeslagen procedure voor het maken van een document schrijven 
In het volgende fragment laat zien hoe de contextobject gebruiken om te communiceren met DocumentDB middelen.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


Deze opgeslagen procedure neemt als invoer documentToCreate, de hoofdtekst van een document wilt maken in de huidige verzameling. Alle dergelijke bewerkingen zijn asynchroon en afhankelijk zijn van de JavaScript-functie terugbellen. De callback-functie heeft twee parameters, één voor het error-object in het geval dat de bewerking is mislukt, en één voor het gemaakte object. Binnen het terugbellen, kunnen gebruikers de uitzondering verwerken of genereert een fout. In het geval een retouraanroep is niet beschikbaar en er een fout opgetreden is, de runtime DocumentDB een fout genereert.   

In het vorige voorbeeld genereert de retouraanroep een fout als de bewerking is mislukt. Anders wordt de id van het gemaakte document ingesteld als de hoofdtekst van het antwoord naar de client. Hier ziet u hoe deze opgeslagen procedure wordt uitgevoerd met de invoerparameters.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
Houd er rekening mee dat deze opgeslagen procedure kan worden gewijzigd voor een array van document-instanties als invoer neemt en ze allemaal in dezelfde uitvoering in plaats van meerdere netwerkaanvragen voor het maken van elk van hen afzonderlijk opgeslagen procedure maken. Dit kan worden gebruikt voor het implementeren van een efficiënte bulk importeur voor DocumentDB (Zie verderop in deze handleiding).   

In het voorbeeld wordt beschreven hoe u opgeslagen procedures aangetoond. Wordt ingegaan op triggers en door gebruiker gedefinieerde functies (UDF's) hoger in de zelfstudie.

## <a name="database-program-transactions"></a>Programma databasetransacties
Transactie in de meeste databases kan worden gedefinieerd als een reeks bewerkingen die worden uitgevoerd als een enkele logische werkeenheid. Elke transactie biedt **zuur garandeert**. ZUUR is een bekende acroniem dat voor vier eigenschappen - atomiciteit, consistentie, isolatie en duurzaamheid staat.  

Kortom, atomisch zorgt ervoor dat alle werkzaamheden binnen een transactie behandeld als één eenheid waar beide alles wordt vastgelegd of geen. Consistentie zorgt ervoor dat de gegevens altijd in een goede interne staat tussen transacties is. Isolatie zorgt ervoor dat er geen twee transacties interfereren – in het algemeen, de meeste commerciële systemen bieden verschillende isolatieniveaus die kunnen worden gebruikt op basis van de behoeften van de toepassing. Duurzaamheid zorgt ervoor dat elke wijziging die doorgevoerd in de database altijd aanwezig zijn.   

JavaScript wordt gehost in DocumentDB, in hetzelfde geheugengebied als de database. Daarom verzoeken gedaan in opgeslagen procedures en triggers in hetzelfde bereik van een databasesessie uitvoeren. Hierdoor kunnen DocumentDB zuur garanderen voor alle bewerkingen die deel van één opgeslagen procedure/trigger uitmaken. Houd rekening met de volgende definitie voor de opgeslagen procedure:

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Deze opgeslagen procedure transacties binnen een game app voor handelsartikelen tussen twee spelers in een enkele bewerking gebruikt. De opgeslagen procedure wordt geprobeerd te lezen van de twee documenten die elk overeenkomen met de speler-id's als een argument doorgegeven. Als beide documenten player worden gevonden, werkt de opgeslagen procedure de documenten door hun artikelen worden verwisseld. Als er fouten langs de manier optreden, genereert deze een JavaScript-uitzondering die impliciet afbreken van de transactie.

Als de collectie van de opgeslagen procedure is geregistreerd tegen een collectie één partitie, is de transactie is binnen het bereik van de docuemnts binnen de collectie. Als de collectie is gepartitioneerd, worden de opgeslagen procedures uitgevoerd in het transactiebereik van een sleutel met één partitie. Elke opgeslagen procedure tot uitvoering van een partitie sleutel die overeenkomt met het bereik dat de transactie moet worden uitgevoerd onder moet opnemen. Zie [DocumentDB partitioneren](documentdb-partition-data.md)voor meer informatie.

### <a name="commit-and-rollback"></a>Commit en rollback
Transacties zijn diep en native geïntegreerd in de DocumentDB van JavaScript-programmeermodel. Binnen een JavaScript-functie worden alle bewerkingen automatisch samengevoegd onder één enkele transactie. Als de JavaScript is voltooid zonder enige uitzondering, worden de bewerkingen in de database doorgevoerd. De instructies "BEGIN TRANSACTION" en "COMMIT TRANSACTION" in relationele databases zijn in feite impliciet in DocumentDB.  
 
Als er een uitzondering die wordt doorgegeven door het script, wordt DocumentDB van JavaScript runtime teruggedraaid de gehele transactie. Zoals in het vorige voorbeeld, is er een uitzondering optreedt effectief gelijk aan een 'transactie ROLLBACK' in DocumentDB.
 
### <a name="data-consistency"></a>Consistentie van de gegevens
Opgeslagen procedures en triggers worden altijd uitgevoerd op de primaire replica van de DocumentDB-collectie. Dit zorgt ervoor dat leesbewerkingen van binnen procedures bieden sterke samenhang opgeslagen. Query's met behulp van door gebruiker gedefinieerde functies kunnen worden uitgevoerd op de primaire of een secundaire replica, maar we garanderen om te voldoen aan het niveau van de gewenste consistentie door het kiezen van de juiste replica.

## <a name="bounded-execution"></a>Begrensde uitvoering
Alle DocumentDB bewerkingen moeten uitvoeren binnen de opgegeven server duur van time-out voor aanvraag. Deze beperking geldt ook voor JavaScript-functies (opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies). Als een bewerking van deze termijn niet wordt voltooid, de transactie teruggedraaid. JavaScript-functies moeten voltooien binnen de vastgestelde termijn of implementeren van een voortzetting op basis model voor het uitvoeren van batch/hervatten.  

Ter vereenvoudiging van de ontwikkeling van opgeslagen procedures en triggers voor het verwerken van termijnen, retourneren alle functies onder het collectieobject (voor het maken, lezen, vervangen en verwijderen van documenten en bijlagen) een Booleaanse waarde die aangeeft of de bewerking wordt voltooid. Als deze waarde false is, is een indicatie dat de termijn verloopt en dat de procedure van uitvoering teruglopen moet.  Bewerkingen in de wachtrij voorafgaand aan de eerste niet-geaccepteerde-bewerking te voltooien als de opgeslagen procedure is voltooid in de tijd en meer verzoeken niet in de wachtrij zijn gegarandeerd.  

JavaScript-functies worden ook begrensd op verbruik. DocumentDB reserves doorvoer per groep op basis van de ingerichte grootte van een database. De doorvoer wordt uitgedrukt in een genormaliseerde eenheid van CPU, geheugen en i/o-verbruik aanvraag eenheden of RUs genoemd. JavaScript-functies kunnen gebruiken om een groot aantal RUs binnen korte tijd en snelheid beperkt als de collectie van de limiet is bereikt, haalt. Resource intensieve opgeslagen procedures kunnen ook in quarantaine om de beschikbaarheid van primitieve databasebewerkingen.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Voorbeeld: Meerdere tegelijk importeren van gegevens in een database-programma
Hieronder volgt een voorbeeld van een opgeslagen procedure die is geschreven voor bulkimport documenten in een collectie. Let op hoe de opgeslagen procedure gebonden uitvoering verwerkt aan de hand van de Boole-waarde retourneert de waarde van createDocument, en gebruikt u vervolgens het aantal documenten dat is ingevoegd in elke aanroep van de opgeslagen procedure te volgen en de uitvoering hervatten in batches.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Databasetriggers
### <a name="database-pre-triggers"></a>Voor databasetriggers
DocumentDB biedt triggers die zijn uitgevoerd of worden geactiveerd door een bewerking in een document. Zo kunt u een trigger voor wanneer u een document maakt – deze voorlopige trigger worden uitgevoerd voordat het document wordt gemaakt. Hier volgt een voorbeeld van hoe pre-triggers kunnen worden gebruikt voor het valideren van de eigenschappen van een document dat wordt gemaakt:

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


En de bijbehorende Node.js registratie voor client-side code voor de trigger:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Triggers vooraf geen alle invoerparameters. Het request-object kan worden gebruikt voor het bewerken van het bericht is gekoppeld aan de bewerking. Hier de voorlopige trigger wordt uitgevoerd met het maken van een document en de berichttekst aanvraag bevat het document in JSON-indeling wordt gemaakt.   

Wanneer triggers zijn geregistreerd, kunnen gebruikers de bewerkingen die kan worden uitgevoerd met opgeven. Deze trigger is gemaakt met TriggerOperation.Create, wat betekent dat het volgende is niet toegestaan.

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Na databasetriggers
Na triggers, zoals vooraf triggers zijn gekoppeld aan een bewerking in een document en alle invoerparameters niet nemen. Ze worden uitgevoerd **nadat** die de bewerking is voltooid en toegang hebben tot het response-bericht dat naar de client wordt verzonden.   

In het volgende voorbeeld ziet u na triggers in actie:

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


De trigger kan worden geregistreerd, zoals in het volgende voorbeeld.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Deze trigger wordt gevraagd om de metagegevensdocument en wordt deze bijgewerkt met informatie over het zojuist gemaakte document.  

Wat is belangrijk om te onthouden is de **transactionele** uitvoering van triggers in DocumentDB. Deze trigger na wordt uitgevoerd als onderdeel van dezelfde transactie als het maken van het oorspronkelijke document. Als we een uitzondering genereren uit de na-trigger (zeg als wij niet de metagegevensdocument bij te werken), de gehele transactie mislukken en worden teruggedraaid. Wordt er geen document gemaakt en wordt een uitzondering geretourneerd.  

##<a id="udf"></a>Door de gebruiker gedefinieerde functies
Door de gebruiker gedefinieerde functies (UDF's) worden gebruikt voor het uitbreiden van de DocumentDB SQL query taal grammatica en aangepaste bedrijfslogica te implementeren. Ze kunnen alleen worden aangeroepen vanuit in query's. Ze hebben geen toegang tot het contextobject en zijn bedoeld voor gebruik als een compute-alleen JavaScript. UDF's kunnen daarom op secundaire replica's van de DocumentDB-service worden uitgevoerd.  
 
In het volgende voorbeeld maakt een UDF om te berekenen op basis van tarieven voor verschillende inkomsten haken inkomstenbelasting en vervolgens binnen een query gebruikt om te zoeken naar alle mensen die meer dan $20.000 belastingen betaalde.

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


De UDF kan vervolgens worden gebruikt in query's zoals in het volgende voorbeeld:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>JavaScript-taal geïntegreerd query API
Naast de uitgifte van query's met behulp van SQL-grammatica door DocumentDB van kunt de server-side-SDK u uitvoeren geoptimaliseerde query's via een fluent interface JavaScript zonder enige kennis van SQL. De query van de JavaScript die API u programmatisch query's bouwen kunt door predikaat functies in de functie chainable aanroept, met een syntaxis die bekend zijn van de ECMAScript5 matrix dient te worden en populaire JavaScript-bibliotheken, zoals lodash. Query's worden geparseerd door de runtime JavaScript kunnen worden uitgevoerd met het efficiënt gebruik maakt van de indexcijfers van de DocumentDB.

> [AZURE.NOTE]`__` (dubbele onderstrepingsteken) is een alias voor `getContext().getCollection()`.
> <br/>
> Met andere woorden, u kunt `__` of `getContext().getCollection()` voor toegang tot de JavaScript API-query.

Ondersteunde functies zijn:
<ul>
<li>
<b>... chain(). waarde ([terugbellen] [, opties])</b>
<ul>
<li>
Aaneengeschakelde gesprek moet worden beëindigd, begint met value().
</li>
</ul>
</li>
<li>
<b>filter (predicateFunction [, opties] [, terugbellen])</b>
<ul>
<li>
De invoer met de predikaat functie geeft als resultaat waar/onwaar in/uit invoerdocumenten filteren in de resulterende set filters. Dit werkt lijkt op een WHERE-component in SQL.
</li>
</ul>
</li>
<li>
<b>kaart (transformationFunction [, opties] [, terugbellen])</b>
<ul>
<li>
Geldt een projectie een transformatiefunctie waarmee u elk item invoer wordt toegewezen aan een JavaScript-object of een waarde gegeven. Dit werkt net als een SELECT-component in SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, opties] [, terugbellen])</b>
<ul>
<li>
Dit is een snelkoppeling naar een kaart die de waarde van één eigenschap uit elk item invoer ophaalt.
</li>
</ul>
</li>
<li>
<b>afvlakken ([isShallow] [, opties] [, terugbellen])</b>
<ul>
<li>
Combineert en matrices van elk item invoer in op één array wordt samengevoegd. Dit werkt net als SelectMany in LINQ.
</li>
</ul>
</li>
<li>
<b>sortBy ([predicate] [, opties] [, terugbellen])</b>
<ul>
<li>
Een nieuwe reeks documenten gesorteerd in de stroom van het invoerdocument in oplopende volgorde met behulp van het predicaat bepaalde documenten produceren. Dit werkt net als een ORDER BY-component in SQL.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predicate] [, opties] [, terugbellen])</b>
<ul>
<li>
Een nieuwe reeks documenten gesorteerd in de stroom van het invoerdocument in aflopende volgorde, met behulp van het predicaat bepaalde documenten produceren. Dit werkt net als een x DESC ORDER BY-component in SQL.
</li>
</ul>
</li>
</ul>


Wanneer binnen predikaat en/of selector functies opgenomen, krijgen automatisch de volgende JavaScript-constructs geoptimaliseerd voor uitvoering rechtstreeks op DocumentDB indices:

* Eenvoudige operatoren: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Letterlijke waarden, waaronder de letterlijke object: {}
* var, retour

De volgende JavaScript-constructs doen niet ophalen geoptimaliseerd voor DocumentDB indices:

* Transportbesturing (bv. Als voor, terwijl)
* Aanroepen van functies

Zie voor meer informatie onze [Server-Side JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Voorbeeld: Een opgeslagen procedure met behulp van de JavaScript API-query schrijven

In het volgende voorbeeld wordt een voorbeeld van hoe de JavaScript-API Query kan worden gebruikt in de context van een opgeslagen procedure. De opgeslagen procedure met deze methode voegt een document, gegeven door een invoerparameter en een metagegevens bijgewerkt document, met behulp van de `__.filter()` methode, met minimaal maxSize en totalSize op basis van de eigenschap size het invoerdocument.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>SQL voor het bedriegen blad van Javascript
De volgende tabel vindt u verschillende SQL-query's en de bijbehorende JavaScript-query's.

Als het document sleutels met SQL-query's (bv. `doc.id`) zijn hoofdlettergevoelig.

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL</th>
<th>JavaScript API-Query</th>
<th>Details</th>
</tr>
<tr>
<td>
<pre>
Selecteer * uit documenten
</pre>
</td>
<td>
<pre>
__.map(Function(doc) {return doc;});
</pre>
</td>
<td>Resultaten in alle documenten (gepagineerd met voortzetting token) is.</td>
</tr>
<tr>
<td>
<pre>
SELECT docs.id, docs.message als msg, docs.actions van documenten
</pre>
</td>
<td>
<pre>
__.map(Function(doc) {terug {id: doc.id, msg: doc.message, acties: doc.actions};});
</pre>
</td>
<td>Projecten met de id, bericht (alias naar msg) en actie van alle documenten.</td>
</tr>
<tr>
<td>
<pre>
Selecteer * uit documenten waar docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.filter(Function(doc) {doc.id retourneren === "X998_Y998;"});
</pre>
</td>
<td>Query's voor documenten met het predikaat: id = "X998_Y998".</td>
</tr>
<tr>
<td>
<pre>
Selecteer * uit documenten waar ARRAY_CONTAINS(docs. Tags, 123)
</pre>
</td>
<td>
<pre>
__.filter(Function(x) {x.Tags retourneren & & x.Tags.indexOf(123) > -1;});
</pre>
</td>
<td>Query's voor documenten met een eigenschap Tags en labels is een matrix met de waarde 123.</td>
</tr>
<tr>
<td>
<pre>
SELECT docs.id, docs.message als msg van documenten waar docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.chain() .filter(function(doc) {doc.id retourneren === "X998_Y998";}) .map(function(doc) {terug {id: doc.id, msg: doc.message};}) .value();
</pre>
</td>
<td>Query's voor documenten met een predikaat, id = "X998_Y998" en vervolgens de id en het bericht (alias naar msg)-projecten.</td>
</tr>
<tr>
<td>
<pre>
Selecteer waarde label uit docs JOIN-code IN documenten. Tags ORDER BY docs._ts
</pre>
</td>
<td>
<pre>
__.chain() .filter(function(doc) {return doc. Labels & & Array.isArray (doc. Tags); }) .sortBy(function(doc) {return doc._ts;}) .pluck("tags") .flatten() .value()
</pre>
</td>
<td>Voor documenten die een matrixeigenschap van een, labels, filters en sorteert de resulterende documenten door de eigenschap _ts timestamp-systeem en projecten + effent de matrix Tags.</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>Runtime-ondersteuning
[DocumentDB JavaScript serverkant SDK](http://azure.github.io/azure-documentdb-js-server/) biedt ondersteuning voor de meeste populaire JavaScript taalfuncties als gestandaardiseerde door [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Beveiliging
JavaScript opgeslagen procedures en triggers zijn sandbox zodat de effecten van een script niet naar de andere lekken kunnen zonder tussenkomst van de transactie snapshot-isolatie op het databaseniveau van de. De runtimeomgevingen zijn samengevoegd, maar van de context gereinigd na elke uitvoering. Zij zijn dus gegarandeerd veilig van eventuele ongewenste neveneffecten van elkaar.

### <a name="pre-compilation"></a>Vóór compilatie
Opgeslagen procedures, triggers en UDF's zijn impliciet voorgecompileerde naar de indeling van de byte-code om te voorkomen dat de compilatie kosten op het moment van elk script aanroepen. Op deze manier aanroepen van opgeslagen procedures zijn snel en hebben een lage voetafdruk.

## <a name="client-sdk-support"></a>Ondersteuning van client-SDK
Naast de [Node.js](documentdb-sdk-node.md) -client ondersteunt DocumentDB [.NET](documentdb-sdk-dotnet.md), [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/)en [Python SDK's](documentdb-sdk-python.md). Opgeslagen procedures, triggers en UDF's kunnen worden gemaakt en uitgevoerd met een van deze SDK's ook. In het volgende voorbeeld ziet u hoe maken en uitvoeren van een opgeslagen procedure met de .NET client. Let op hoe de typen .NET worden doorgegeven aan de opgeslagen procedure als JSON en voorgelezen.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


In dit voorbeeld ziet u hoe met behulp van de [SDK voor .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx) voorafgaand trigger maken en maak een document met de trigger is ingeschakeld. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


En in het volgende voorbeeld ziet u hoe een gebruiker gedefinieerde functie (UDF) maken en gebruiken in een [DocumentDB SQL-query](documentdb-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST-API
Alle DocumentDB bewerkingen kunnen worden uitgevoerd op een wijze die RESTful. Opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies kunnen worden geregistreerd onder een collectie met behulp van HTTP POST. Hier volgt een voorbeeld van het registreren van een opgeslagen procedure:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


De opgeslagen procedure wordt geregistreerd door het uitvoeren van een POST-aanvraag ten opzichte van de URI dbs/testdb/colls/testColl/sprocs bij de instantie die de opgeslagen procedure maken. Triggers en UDF's kunnen ook worden geregistreerd door een POST tegen/triggers en /udfs respectievelijk.
Deze opgeslagen procedure kan vervolgens worden uitgevoerd via de uitgifte van een POST-aanvraag ten opzichte van de koppeling bron:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Hier worden de invoer voor de opgeslagen procedure wordt doorgegeven in het hoofdgedeelte van de aanvraag. Houd er rekening mee dat de invoer wordt doorgegeven als een matrix JSON van invoerparameters. De opgeslagen procedure wordt de eerste invoer als een document dat een instantie van het antwoord. Het antwoord dat is ontvangen, is als volgt:

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


In tegenstelling tot de opgeslagen procedures, triggers, kunnen niet rechtstreeks worden uitgevoerd. In plaats daarvan worden uitgevoerd als onderdeel van een bewerking in een document. We kunt de triggers worden uitgevoerd met een verzoek met behulp van HTTP-headers opgeven. Het volgende is een verzoek om een document te maken.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Hier is de voorlopige trigger worden uitgevoerd met de aanvraag opgegeven in de koptekst van de x-ms-documentdb-pre-trigger-include. Dienovereenkomstig, krijgen alle triggers na in de kop van de x-ms-documentdb-post-trigger-include. Houd er rekening mee dat beide vóór en na triggers voor een bepaalde aanvraag kunnen worden opgegeven.

## <a name="sample-code"></a>Voorbeeld van code

Server-side codevoorbeelden (met inbegrip van [bulk verwijderen](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)en [bijwerken](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) kunt u vinden in onze [opslagplaats Github](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Wilt u uw Meester opgeslagen procedure delen? Stuur ons een pull-aanvraag! 

## <a name="next-steps"></a>Volgende stappen

Wanneer u een of meer opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies gemaakt hebt, kunt u deze laden en weergeven in de Explorer Script met Azure-Portal. Zie voor meer informatie, [weergaven, opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies met het Script DocumentDB Explorer](documentdb-view-scripts.md).

Mogelijk ook nuttig voor u de volgende verwijzingen en bronnen in het pad voor meer informatie over het programmeren van DocumentDB aan de serverzijde:

- [SDK's Azure DocumentDB](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [JavaScript: JSON typesysteem](http://www.json.org/js.html) 
- [Veilige en draagbare Database uitbreiden](http://dl.acm.org/citation.cfm?id=276339) 
- [Service-Oriented Database-architectuur](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [De .NET Runtime in Microsoft SQL server die als host fungeert](http://dl.acm.org/citation.cfm?id=1007669)
