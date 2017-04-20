##<a name="create-client"></a>Een clientverbinding maakt

Een clientverbinding maakt met het maken van een `WindowsAzure.MobileServiceClient` object.  Vervangen `appUrl` door de URL naar uw mobiele App.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

##<a name="table-reference"></a>Werken met tabellen

Access- of als u gegevens wilt maken van een verwijzing naar de back-end-tabel. Vervangen `tableName` met de naam van de tabel

```
var table = client.getTable(tableName);
```

Zodra u een tabel verwijst hebt, werkt u verder met de tabel:

* [Een tabel](#querying)
  * [Filteren van gegevens](#table-filter)
  * [Paginering van gegevens](#table-paging)
  * [Gegevens sorteren](#sorting-data)
* [Gegevens invoegen](#inserting)
* [Wijzigen van gegevens](#modifying)
* [Gegevens verwijderen](#deleting)

###<a name="querying"></a>Procedure: een tabelverwijzing een Query

Nadat u een tabelverwijzing hebt, kunt u deze gegevens op de server worden opgevraagd.  Query's worden gemaakt in een taal "LINQ-achtige".
Als u wilt alle gegevens uit de tabel, gebruikt u de volgende:

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

De functie met succes wordt aangeroepen met de resultaten.   Gebruik geen `for (var i in results)` in het succes zoals die wordt herhaald over de informatie die is opgenomen in de resultaten wanneer andere queryfuncties (zoals `.includeTotalCount()`) worden gebruikt.

Raadpleeg voor meer informatie over de syntaxis van de Query, de [Query objectdocumentatie].

####<a name="table-filter"></a>Filteren van gegevens op de server

U kunt een `where` component van de tabelverwijzing:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

U kunt ook een functie waarmee het object worden gefilterd.  In dit geval de `this` variabele wordt toegewezen aan het huidige object worden gefilterd.  Het volgende is functioneel gelijk aan het vorige voorbeeld:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

####<a name="table-paging"></a>Paginering van gegevens

Gebruik de methoden take() en skip().  Bijvoorbeeld als u wilt dat de tabel te splitsen in een rij 100 records:

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

De `.includeTotalCount()` -methode gebruikt om een veld totalCount toevoegen aan het object resultaten.  Het veld totalCount wordt gevuld met het totale aantal records dat wordt geretourneerd als er geen paginering wordt gebruikt.

U kunt de variabele pagina's en enkele knoppen gebruikersinterface bieden een lijst met pagina; Gebruik loadPage() om te laden de nieuwe records voor elke pagina.  U te zonnestelsels van snelheid toegang tot records die al zijn geladen in het cachegeheugen implementeren.


####<a name="sorting-data"></a>Procedure: gegevens gesorteerd

Gebruik de methoden voor .orderBy() of .orderByDescending()-query:

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Raadpleeg voor meer informatie over de Query-object, de [Query objectdocumentatie].

###<a name="inserting"></a>Procedure: gegevens invoegen

Een JavaScript-object met de juiste datum en table.insert() asynchroon:

```
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Op de succesvolle plaatsingskosten, het ingevoegde object geretourneerd met de extra velden die nodig voor de synchronisatie zijn.  Met deze gegevens voor latere updates moet u uw eigen cache bijwerken.

Houd er rekening mee dat de Azure Mobile Apps Node.js Server SDK dynamische schema voor ontwikkelingsdoeleinden ondersteunt.
Het schema van de tabel wordt bijgewerkt voor dynamische schema, onderweg, zodat u kunt kolommen aan de tabel toevoegen door ze op te geven in een insert of update-bewerking.  Wij raden aan dat u dynamische schema uitschakelen voordat u uw toepassing op de productie.

###<a name="modifying"></a>Procedure: gegevens wijzigen

Vergelijkbaar met de .insert()-methode, u moet een Update object maken en roept .update().  De ID van de record worden bijgewerkt door het update-object moet bevatten: dit is verkregen bij het lezen van de record of bij het aanroepen van .insert().

```
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

###<a name="deleting"></a>Procedure: gegevens verwijderen

Roep de methode .del() om een record te verwijderen.  De ID doorgeeft in de verwijzing naar een object:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
