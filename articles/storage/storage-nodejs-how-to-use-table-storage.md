<properties
    pageTitle="Het gebruik van opslag van Node.js Azure tabel | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud opslag Azure-tabel, een gegevensarchief NoSQL."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Het gebruik van opslag van Node.js Azure tabel

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Dit onderwerp bevat algemene scenario's voor het gebruik van de service Azure-tabel in een toepassing Node.js uitvoeren.

De codevoorbeelden in dit onderwerp wordt ervan uitgegaan dat er al een toepassing Node.js. Zie voor meer informatie over het maken van een toepassing Node.js in Azure, een van de volgende onderwerpen:

- [Een Node.js web app in Azure App Service maken](../app-service-web/web-sites-nodejs-develop-deploy-mac.md)
- [Bouwen en implementeren van een Node.js web app met Azure met WebMatrix](../app-service-web/web-sites-nodejs-use-webmatrix.md)
- [Bouwen en implementeren van een toepassing Node.js een Azure Cloud-service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (met behulp van Windows PowerShell)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="configure-your-application-to-access-azure-storage"></a>Uw toepassing configureren voor toegang tot opslag Azure

Azure opslag gebruikt, moet u de SDK Azure opslag voor Node.js, inclusief een set met gemak bibliotheken die met de REST opslagservices communiceren.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Knooppunt Package Manager (NPM) gebruiken om het pakket te installeren

1.  Gebruik een opdrachtregelinterface zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix) en navigeer naar de map waar u de toepassing hebt gemaakt.

2.  Typ **npm azure opslag installeren** in het opdrachtvenster. Uitvoer van de opdracht is vergelijkbaar met het volgende voorbeeld.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  U kunt handmatig uitvoeren met de opdracht **ls** te controleren dat een **knooppunt\_modules** map is gemaakt. In deze map vindt u de **azure opslag** -pakket de bibliotheken die u nodig hebt bevat voor toegang tot opslag.

### <a name="import-the-package"></a>Het pakket importeren

Voeg de volgende code boven aan het bestand **server.js** in uw toepassing:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Een verbinding Azure opslag instellen

De module azure leest de omgevingsvariabelen AZURE\_opslag\_ACCOUNT en AZURE\_opslag\_ACCESS\_sleutel of AZURE\_opslag\_verbinding\_STRING voor de informatie die nodig is om verbinding met uw account Azure opslag. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens opgeven bij het aanroepen van **TableService**.

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in [Azure Portal](https://portal.azure.com) voor een Website Azure [Node.js web app met behulp van de tabel Azure Service].

## <a name="create-a-table"></a>Een tabel maken

De volgende code wordt een **TableService** -object gemaakt en gebruikt om een nieuwe tabel te maken. Voeg de volgende aan de bovenkant van de **server.js**.

    var tableSvc = azure.createTableService();

De aanroep **createTableIfNotExists** maakt u een nieuwe tabel met de opgegeven naam als deze nog niet bestaat. In het volgende voorbeeld wordt een nieuwe tabel MijnTabel '' als deze nog niet bestaat.

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
      if(!error){
        // Table exists or created
      }
    });

De `result.created` zijn `true` als een nieuwe tabel wordt gemaakt, en `false` als de tabel al bestaat. De `response` bevat informatie over de aanvraag.

### <a name="filters"></a>Filters

Optionele filterbewerkingen kunnen worden toegepast op de bewerkingen die worden uitgevoerd met behulp van **TableService**. Filteren van bewerkingen kunt opnemen, aanmelden, automatisch opnieuw wordt geprobeerd, enz. Filters zijn objecten die een methode met de handtekening te implementeren:

    function handle (requestOptions, next)

Na de voorverwerking van de opties van de aanvraag, de methode moet aanroepen 'volgende' geven een retouraanroep met de volgende handtekening:

    function (returnObject, finalCallback, next)

In dit mechanisme, en na de verwerking van de returnObject (de reactie van de aanvraag naar de server), moet de retouraanroep op volgende aanroepen als deze bestaat als u wilt doorgaan met het verwerken van andere filters of roepen finalCallback gewoon anders om het oproepen van de service beëindigen.

Twee filters die logica opnieuw implementeren zijn opgenomen in de SDK Azure voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende manier maakt een **TableService** -object dat de **ExponentialRetryPolicyFilter**wordt gebruikt:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## <a name="add-an-entity-to-a-table"></a>Een entiteit aan een tabel toevoegen

Als u wilt toevoegen van een entiteit, eerst een object waarin u de entiteitseigenschappen voor de maken. Alle entiteiten bevat een **PartitionKey** en **RowKey**, die de unieke id voor de entiteit.

* **PartitionKey** - bepaalt de partitie die de entiteit worden opgeslagen in

* **RowKey** - aanduiding een unieke voor de entiteit in de partitie

Zowel **PartitionKey** als **RowKey** moet tekenreekswaarden. Zie [Wat zijn de gegevens van tabel servicemodel](http://msdn.microsoft.com/library/azure/dd179338.aspx)voor meer informatie.

Hier volgt een voorbeeld van een entiteit te definiëren. Houd er rekening mee dat **dueDate** wordt gedefinieerd als een soort **Edm.DateTime**. Het type op te geven is optioneel en typen worden afgeleid als het niet opgegeven.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE] Er is ook **een tijdstempelveld voor elke record die wordt ingesteld door Azure wanneer een entiteit wordt ingevoegd of bijgewerkt** .

U kunt ook de **entityGenerator** entiteiten maken. Het volgende voorbeeld wordt dezelfde taak entiteit met behulp van de **entityGenerator**.

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

Als een entiteit toevoegen aan uw tabel, geeft u de entiteitsobject aan de methode **insertEntity** .

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
      if(!error){
        // Entity inserted
      }
    });

Als de bewerking voltooid is, `result` , bevat de [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) van de ingevoegde record en `response` bevat informatie over de bewerking.

Voorbeeld van de reactie:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] Standaard **insertEntity** resulteert niet in de ingevoegde entiteit als onderdeel van de `response` informatie. Als u van plan bent andere bewerkingen op deze entiteit of wilt de gegevens in de cache, kan het handig zijn om deze geretourneerd als onderdeel van de `result`. U kunt dit doen door de **echoContent** als volgt inschakelen:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## <a name="update-an-entity"></a>Een entiteit bijwerken

Er zijn meerdere methoden voor het bijwerken van een bestaande entiteit:

* **replaceEntity** - een bestaande entiteit worden bijgewerkt door deze te vervangen

* **mergeEntity** - een bestaande entiteit bijgewerkt met nieuwe waarden samenvoegen in bestaande entiteit

* een bestaande entiteit **insertOrReplaceEntity** - bijgewerkt door deze te vervangen. Er bestaat geen entiteit, een nieuw ingevoegd

* **insertOrMergeEntity** - werkt een bestaande entiteit door nieuwe waarden in de bestaande samen te voegen. Er bestaat geen entiteit, een nieuw ingevoegd

Het volgende voorbeeld wordt het bijwerken van een entiteit met **replaceEntity**:

    tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] Standaard wordt bijwerken van een entiteit niet gecontroleerd of de gegevens worden bijgewerkt eerder is gewijzigd door een ander proces. Gelijktijdige updates ondersteunen:
>
> 1. Krijg de ETag van het object wordt bijgewerkt. Dit wordt geretourneerd als onderdeel van de `response` voor een entiteit bewerking gerelateerde en kunnen worden opgehaald met `response['.metadata'].etag`.
>
> 2. Bij het uitvoeren van een bijwerkbewerking op een entiteit ETag gegevens eerder overgenomen voor de nieuwe entiteit toevoegen. Bijvoorbeeld:
>
>     `entity2['.metadata'].etag = currentEtag;`
>
> 3. De updatebewerking uitvoeren. Als de entiteit is gewijzigd sinds u de ETag-waarde opgehaald zoals een ander exemplaar van de toepassing, een `error` wordt geretourneerd met de melding dat de update voorwaarde opgegeven in de aanvraag niet is voldaan.

Met **replaceEntity** en **mergeEntity**, als de entiteit die wordt bijgewerkt, niet bestaat, mislukt de updatebewerking. Dus als u wilt dat voor het opslaan van een entiteit, ongeacht of deze al bestaat, gebruikt u **insertOrReplaceEntity** of **insertOrMergeEntity**.

De `result` voor een succesvolle update bevat bewerkingen de **Etag** van de bijgewerkte entiteit.

## <a name="work-with-groups-of-entities"></a>Werken met groepen van entiteiten

Soms is het verstandig meerdere bewerkingen samen in een batch zodat atomaire verwerkt door de server te verzenden. U doet dat door de klasse **TableBatch** u een batch wilt maken en gebruik vervolgens de methode **executeBatch** van **TableService** de batch-bewerkingen uit te voeren.

 In het volgende voorbeeld ziet u twee entiteiten in een batch indienen:

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

Voor succesvolle batchbewerkingen, `result` bevat informatie voor elke bewerking in de batch.

### <a name="work-with-batched-operations"></a>Werken met batchbewerkingen

Bewerkingen die worden toegevoegd aan een batch kunnen worden gekeurd door bekijken de `operations` eigenschap. U kunt ook de volgende methoden om te werken met bewerkingen:

* **wissen** - Hiermee wist u alle bewerkingen van een batch

* **getOperations** - met deze eigenschap wordt een bewerking van de partij

* **hasOperations** - geeft als resultaat waar als de batch bevat bewerkingen

* **removeOperations** - verwijdert een bewerking

* **grootte** - geeft als resultaat het aantal bewerkingen in de batch

## <a name="retrieve-an-entity-by-key"></a>Een entiteit met sleutel ophalen

U kunt een specifieke entiteit op basis van de **PartitionKey** en **RowKey**gebruiken de methode **retrieveEntity** .

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

Wanneer deze bewerking voltooid is, `result` , bevat de entiteit.

## <a name="query-a-set-of-entities"></a>Een verzameling van entiteiten query

Gebruiken om een query uitvoert op een tabel, het **TableQuery** -object te maken van een query-expressie met de volgende componenten:

* **Selecteer** - de velden van de query worden geretourneerd

* **waar** - waar u de component

    * **en** - een `and` where-voorwaarde

    * **of** - een `or` where-voorwaarde

* **top** - het aantal artikelen op te halen


Het volgende voorbeeld wordt een query die de bovenste vijf items met een PartitionKey van 'hometasks'.

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

Aangezien **selecteren** niet wordt gebruikt, worden alle velden worden teruggestuurd. Gebruik **queryEntities**voor het uitvoeren van de query op basis van een tabel. In het volgende voorbeeld retourneert entiteiten uit "mytable" deze query.

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

Als dit lukt, `result.entries` bevat een matrix van entiteiten die aan de query. Als de query niet kan retourneren van alle entiteiten, `result.continuationToken` niet -*null* en kan worden gebruikt als de derde parameter van **queryEntities** om meer resultaten te halen. Voor de oorspronkelijke query, gebruikt u *null* voor de derde parameter.

### <a name="query-a-subset-of-entity-properties"></a>Een subset van de entiteitseigenschappen opvragen

Een query in een tabel kunt u een paar velden ophalen uit een entiteit.
Dit vermindert de bandbreedte en kan query's sneller, met name voor grote entiteiten. Gebruik de component **selecteren** en geeft u de namen van de velden die moeten worden geretourneerd. De volgende query wordt bijvoorbeeld alleen de velden **Omschrijving** en **dueDate** terug.

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## <a name="delete-an-entity"></a>Een entiteit verwijderen

U kunt een entiteit met de sleutels die partitie en rij verwijderen. In het volgende voorbeeld bevat het object **task1** de waarden **RowKey** en **PartitionKey** van de entiteit moet worden verwijderd. Het object wordt vervolgens doorgegeven aan de methode **deleteEntity** .

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] Overweeg het gebruik van ETags bij het verwijderen van items, om ervoor te zorgen dat het item niet is gewijzigd door een ander proces. Zie [een entiteit bijwerken](#update-an-entity) voor informatie over het gebruik van ETags.

## <a name="delete-a-table"></a>Een tabel verwijderen

De volgende code verwijdert een tabel uit een opslag-account.

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

Gebruik **deleteTableIfExists**als u niet zeker weet of de tabel bestaat.

## <a name="use-continuation-tokens"></a>Voortzetting tokens gebruiken

Wanneer u tabellen voor grote hoeveelheden resultaten zoeken wilt, zoeken naar voortzetting tokens. Kunnen er grote hoeveelheden gegevens beschikbaar zijn voor uw query die u niet weet misschien als u niet maken kan om te herkennen als een voortzetting-token aanwezig is.

De resultaten-object als resultaat gegeven tijdens het opvragen van entiteiten wordt een `continuationToken` eigenschap wanneer deze een token aanwezig is. Vervolgens kunt u dit bij het uitvoeren van een query te verplaatsen over de diensten en de partitie tabel.

Wanneer u een query uitvoert, kan een parameter continuationToken tussen het query-object en de callback-functie worden geleverd:

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Als u de `continuationToken` -object vindt u eigenschappen, zoals `nextPartitionKey`, `nextRowKey` en `targetLocation`, die kan worden gebruikt om alle resultaten doorlopen.

Er is ook een voorbeeld van een voortzetting binnen de Azure opslag Node.js repo op GitHub. Zoek naar `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Werken met gedeelde toegang handtekeningen

Gedeelde toegang handtekeningen (SAS) zijn een veilige manier voor gedetailleerde toegang tot tabellen zonder dat u uw accountnaam voor opslag of sleutels. SA's worden vaak gebruikt om beperkte toegang tot uw gegevens, zoals een queryrecords-mobiele toepassingen.

Een vertrouwde toepassing, zoals een cloud-gebaseerde service genereert een SAS met behulp van de **generateSharedAccessSignature** van de **TableService**en biedt dit aan een niet-vertrouwde of semi-vertrouwde toepassing, zoals een mobiele app. De SAS wordt gegenereerd op basis van een beleid waarin de begin- en einddatums waarvan de SA's geldig is als het toegangsniveau verleend aan de houder SAS.

In het volgende voorbeeld genereert een nieuwe gedeelde-beleid waarmee de houder SAS ('r') in de tabel query en verloopt de 100 minuten na het tijdstip waarop dat deze is gemaakt.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

Houd er rekening mee dat de moet hostinformatie worden verstrekt, zoals vereist is wanneer de houder SAS probeert toegang te krijgen tot de tabel.

Vervolgens de clienttoepassing gebruikmaakt van de SA's met **TableServiceWithSAS** voor het uitvoeren van bewerkingen op de tabel. In het volgende voorbeeld maakt verbinding met de tabel en een query uitvoeren.

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

Aangezien de SA's is gegenereerd met alleen query toegang als een poging wilt invoegen, bijwerken of verwijderen entiteiten zijn aangebracht, wordt een fout geretourneerd.

### <a name="access-control-lists"></a>Access Control Lists

U kunt ook een lijst met ACL (Access Control) het-beleid instellen voor een SA's. Dit is handig als u dat meerdere clients toegang krijgen tot de tabel wilt, maar bieden verschillende-beleid voor elke client.

Een ACL is geïmplementeerd met behulp van een array-beleid, met een ID die is gekoppeld aan elk beleid. In het volgende voorbeeld definieert twee beleidsregels, één voor 'user1' en voor 'gebruiker2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

In het volgende voorbeeld wordt de huidige ACL voor de **hometasks** -tabel en wordt het nieuwe beleid met behulp van **setTableAcl**. Deze benadering kunt:

    var extend = require('extend');
    tableSvc.getTableAcl('hometasks', function(error, result, response) {
    if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Zodra de ACL is ingesteld, kunt u vervolgens een SAS op basis van de ID voor een beleid. Het volgende voorbeeld wordt een nieuwe SA's voor 'gebruiker2':

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie.

-   [Azure Storage teamblog][].
-   [Azure opslag SDK voor knooppunt][] opslagplaats op GitHub.
-   [Node.js Developer Center](/develop/nodejs/)

  [Azure opslag SDK voor knooppunt]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Azure opslag-teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js web app met behulp van de tabel Azure Service]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md
