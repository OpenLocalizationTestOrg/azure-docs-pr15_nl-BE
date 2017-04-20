<properties 
    pageTitle="SQL-syntaxis en SQL-query voor DocumentDB | Microsoft Azure" 
    description="Meer informatie over de SQL-syntaxis, basisbegrippen in database en SQL-query's voor DocumentDB, een database NoSQL. SQL kan worden gebruikt als een querytaal JSON in DocumentDB." 
    keywords="SQL-syntaxis, sql-query, sql-query's, query-taal json, basisbegrippen in database en sql-query's, statistische functies"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="sql-query-and-sql-syntax-in-documentdb"></a>SQL-query en de SQL-syntaxis in DocumentDB
DocumentDB voor Microsoft Azure documenten opvragen met behulp van SQL (Structured Query Language) als een querytaal JSON. DocumentDB is echt schema vrij. Op grond van haar inzet voor het gegevensmodel JSON direct in de database-engine biedt automatische indexering van JSON-documenten zonder expliciete schema of secundaire indexen worden gemaakt. 

We hadden twee doelen in gedachten bij het ontwerp van de querytaal voor DocumentDB:

-   We wilden in plaats van een nieuwe querytaal voor JSON vruchtbare, ondersteuning voor SQL. SQL is een van de meest vertrouwde en populaire querytalen. DocumentDB SQL biedt een formele programmeermodel voor uitgebreide query's via JSON-documenten.
-   We wilden een JSON-document database kan worden uitgevoerd JavaScript rechtstreeks in de database-engine, programmeermodel van JavaScript gebruikt als basis voor onze querytaal. De SQL-DocumentDB zijn basis heeft in het systeem van JavaScript, evaluatie van de expressie en functie-aanroep. Deze draai biedt een natuurlijke programmeermodel voor relationele projecties, hiërarchische navigatie tussen JSON-documenten, self joins, ruimtelijke query's, en het aanroepen van de gebruiker gedefinieerde functies (UDF's), geschreven in JavaScript, onder andere functies volledig. 

Wij geloven dat deze mogelijkheden zijn de sleutel tot vermindering van de wrijving tussen de toepassing en de database en zijn van cruciaal belang voor de productiviteit van ontwikkelaars.

Het is raadzaam om aan de slag door het bekijken van de volgende video, waar Aravind Ramachandran mogelijkheden voor het opvragen van DocumentDB bevat, en via onze [Speelplaats Query](http://www.documentdb.com/sql/demo), kunt u uitproberen DocumentDB en SQL-query's uitvoeren op onze dataset.

> [AZURE.VIDEO dataexposedqueryingdocumentdb]

Vervolgens terug naar dit artikel, waarbij we beginnen met een SQL-query zelfstudie die u enkele eenvoudige JSON-documenten en SQL-opdrachten doorloopt.

## <a name="getting-started-with-sql-commands-in-documentdb"></a>Aan de slag met SQL-opdrachten in DocumentDB
Overzicht SQL DocumentDB op het werk, laten we beginnen met een paar eenvoudige JSON-documenten en wandel door enkele eenvoudige query's tegen. U kunt deze twee JSON-documenten over twee families. Houd er rekening mee dat met DocumentDB, we niet hoeft alle schema's of secundaire indices expliciet maken. We moeten gewoon de JSON-documenten aan een collectie DocumentDB invoegen en vervolgens een query. Hier hebben we een eenvoudige JSON document voor de familie Andersen, de ouders, kinderen (en hun huisdieren), en de registratie-informatie. Het document bevat tekenreeksen, getallen, booleans, matrices en geneste eigenschappen. 

**Document**  

    {
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }


Hier volgt een tweede document met een subtiel verschil: `givenName` en `familyName` worden gebruikt in plaats van `firstName` en `lastName`.

**Document**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                 "givenName": "Lisa", 
                 "gender": "female", 
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "creationDate": 1431620462,
        "isRegistered": false
    }



Nu gaan we proberen enkele query's op deze gegevens om te begrijpen dat sommige van de belangrijkste aspecten van SQL DocumentDB. Bijvoorbeeld de volgende query retourneert de documenten waarbij het veld-id overeenkomt met `AndersenFamily`. Omdat het een `SELECT *`, de uitvoer van de query is het volledige JSON-document:

**Query**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Bekijk nu het geval waar we moeten de JSON-uitvoer in een andere shape opmaken. Deze query projecten met twee geselecteerde velden, de naam en plaats een nieuw JSON-object wanneer het adres plaats dezelfde naam als de staat heeft. In dit geval "NY, NY" komt overeen met.

**Query**   

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Resultaten**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


De volgende query geeft als resultaat de opgegeven namen van kinderen in de familie waarvan de id overeenkomt met `WakefieldFamily` besteld op de plaats van verblijf.

**Query**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Resultaten**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Wij willen graag de aandacht vestigen op een aantal opmerkelijk aspecten van de querytaal DocumentDB via de voorbeelden die we tot nu toe hebt gezien:  
 
-   Sinds de DocumentDB SQL JSON-waarden, behandelt structuur in de vorm van diensten in plaats van rijen en kolommen. Daarom de taal kunt u verwijzen naar de knooppunten van de structuur op een willekeurige diepte, zoals `Node1.Node2.Node3…..Nodem`, vergelijkbaar met een relationele SQL die verwijzen naar de verwijzing deel twee van `<table>.<column>`.   
-   De structured query language werkt-schema minder gegevens. Daarom moet het systeem dynamisch worden verbonden. De opbrengst van dezelfde expressie kan verschillende typen op verschillende documenten. Het resultaat van een query is een geldige waarde voor JSON, maar is niet gegarandeerd van een vast schema.  
-   DocumentDB ondersteunt alleen strikte JSON-documenten. Dit betekent dat het typesysteem en expressies zijn beperkt tot verwerken alleen JSON. Raadpleeg de [JSON-specificatie](http://www.json.org/) voor meer informatie.  
-   Een collectie DocumentDB is een container schema vrij van JSON-documenten. De relaties in gegevensentiteiten binnen en tussen documenten in een collectie worden impliciet vastgelegd door insluiting en niet door de primaire sleutel en refererende sleutel betrekkingen. Dit is een belangrijk aspect ongepast in het licht van de intra-document joins die verderop in dit artikel beschreven.

## <a name="documentdb-indexing"></a>Indexering van DocumentDB

Voordat we in de DocumentDB SQL-syntaxis ophalen, is het waard het indexeren ontwerp in DocumentDB. 

Het doel van de database-indexen is voor query's in hun diverse vormen en vormen met minimale verbruik (zoals CPU en input/output) en goede doorvoer en lage latentie te bieden. De keuze van de juiste index voor query's in een database is vaak veel planning en experimenten. Deze aanpak vormt een uitdaging voor de schema-less databases waarbij de gegevens niet voldoen aan een strikte schema en snel ontwikkeld. 

Wanneer we het subsysteem indexing DocumentDB ontworpen, stel we daarom de volgende doelstellingen:

-   Documenten indexeren zonder schema: het indexeren subsysteem vereist een schema-informatie of veronderstellingen over het schema van de documenten. 

-   Ondersteuning voor efficiënte, uitgebreide hiërarchische en relationele query's: de index ondersteunt de querytaal DocumentDB efficiënt, inclusief ondersteuning voor hiërarchische en relationele projecties.

-   Ondersteuning voor een consistente query's in face of een continue hoeveelheid schrijft: voor het schrijven van hoge doorvoer werkbelasting met consistente query's, de index bijgewerkt stapsgewijs, efficiënt en online tegen een aanhoudende volume schrijft. De consistente index-update is van cruciaal belang voor de query's op het niveau van samenhang waarin de gebruiker het documentservice geconfigureerd.

-   Ondersteuning voor meerdere pacht: gegeven de reservering gebaseerd model voor resource governance over huurders, index-updates worden uitgevoerd binnen het budget van systeembronnen (CPU, geheugen en i/o-bewerkingen per seconde) die per replica worden toegewezen. 

-   Efficiëntie van de opslag: voor de kosteneffectiviteit, de opslagoverhead op schijf van de index is gebonden en voorspelbaar. Dit is essentieel, omdat het DocumentDB kan de ontwikkelaar te maken op basis van kosten wisselwerking tussen index overhead in verhouding tot de prestaties van query's.  

Raadpleeg de [DocumentDB monsters](https://github.com/Azure/azure-documentdb-net) op MSDN voor voorbeelden van het configureren van de indexing-beleid voor een verzameling. Laten we nu krijgen in de details van de DocumentDB SQL-syntaxis.


## <a name="basics-of-a-documentdb-sql-query"></a>Basisbeginselen van een DocumentDB SQL-query
Elke query bestaat uit een SELECT-component en een optionele FROM en WHERE-componenten per ANSI SQL-normen. Normaal gesproken voor elke query, moet de bron in de FROM-component wordt geïnventariseerd. Het filter in de WHERE-component wordt vervolgens toegepast op de bron op te halen van een subset van JSON-documenten. Ten slotte wordt de SELECT-component gebruikt om de gevraagde JSON-waarden in de lijst selecteren.
    
    SELECT [TOP <top_expression>] <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a name="from-clause"></a>FROM-component
De `FROM <from_specification>` -component is optioneel, tenzij de bron is gefilterd of verderop in de query geschatte. Het doel van deze clausule is om op te geven van de gegevensbron waarop de query moet worden uitgevoerd. De hele collectie is vaak de bron, maar een kunt in plaats daarvan een subset van de collectie opgeven. 

Een query, zoals `SELECT * FROM Families` geeft aan dat de volledige collectie van de Families de bron op die u is wilt inventariseren. Een speciale basis-id kan worden gebruikt om de collectie in plaats van de naam van de collectie vertegenwoordigt. De volgende lijst bevat de regels die per query gelden:

- De collectie is alias, zoals `SELECT f.id FROM Families AS f` of `SELECT f.id FROM Families f`. Hier `f` is het equivalent van `Families`. `AS`is een optioneel trefwoord tot de alias van de id.

-   Opmerking dat eenmaal alias, de originele bron kan niet worden gebonden. Bijvoorbeeld, `SELECT Families.id FROM Families f` de syntaxis is ongeldig omdat de id "Families" kan niet meer worden omgezet.

-   Alle eigenschappen die moeten worden verwezen moet volledig gekwalificeerd zijn. Bij gebrek aan strikte schema hechting is dit om te voorkomen dat een dubbelzinnige bindingen afgedwongen. Daarom, `SELECT id FROM Families f` de syntaxis is ongeldig omdat de eigenschap `id` niet is gebonden.
    
### <a name="sub-documents"></a>Onderliggende documenten
De bron kan ook worden verminderd tot een kleiner gedeelte. Bijvoorbeeld voor het opsommen van een substructuur in elk document kan de hoofdmap met submappen vervolgens worden de bron, zoals in het volgende voorbeeld.

**Query**

    SELECT * 
    FROM Families.children

**Resultaten**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Terwijl het bovenstaande voorbeeld wordt een matrix als bron gebruikt, kan een object ook worden gebruikt als bron is wat in het volgende voorbeeld wordt weergegeven. Elke geldige JSON waarde (geen niet-gedefinieerd) die kan worden gevonden in de bron wordt beschouwd voor de opneming in het resultaat van de query. Als geen enkele families een `address.state` waarde, ze worden uitgesloten van het resultaat van de query.

**Query**

    SELECT * 
    FROM Families.address.state

**Resultaten**

    [
      "WA", 
      "NY"
    ]


## <a name="where-clause"></a>WHERE-component
De WHERE-component (**`WHERE <filter_condition>`**) is optioneel. Hiermee geeft u aan de voorwaarden die de JSON-documenten in de bron moet voldoen om te worden opgenomen als onderdeel van het resultaat. Een JSON-document, moet de opgegeven voorwaarden op "true" moet worden beschouwd voor het resultaat opleveren. De WHERE-component wordt gebruikt door de index laag om de absolute kleinste subset van brondocumenten die onderdeel van het resultaat uitmaken vast te stellen. 

De volgende query vraagt om documenten met een van de naameigenschap waarvan de waarde `AndersenFamily`. Andere documenten waarvoor een eigenschap name of waar de waarde niet overeenkomt met `AndersenFamily` is uitgesloten. 

**Query**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Het vorige voorbeeld blijkt een eenvoudige gelijkheid query. DocumentDB SQL ondersteunt ook een groot aantal scalaire expressies. De meest gebruikte zijn binair en unaire expressies. Verwijzen naar eigenschappen van het bronobject JSON zijn ook toegestaan. 

De volgende binaire operatoren worden ondersteund en kunnen worden gebruikt in query's, zoals in de volgende voorbeelden:  
<table>
<tr>
<td>Rekenkundige operatoren</td> 
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitsgewijs</td>    
<td>|, &, ^, <<, >>, >>> (shift-rechts nul opvulling) </td>
</tr>
<tr>
<td>Logische</td>
<td>EN, OF NIET</td>
</tr>
<tr>
<td>Vergelijking</td> 
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Tekenreeks</td> 
<td>|| (samenvoegen)</td>
</tr>
</table>  

Laten we op sommige query's met behulp van binaire operatoren.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1
    
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5
    
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Unaire operatoren +,-, ~ niet worden ondersteund en kunnen worden gebruikt in query's, zoals in het volgende voorbeeld wordt getoond:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1
    
    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Eigenschap verwijzingen zijn naast de binaire bestanden en unaire operatoren, ook toegestaan. Bijvoorbeeld, `SELECT * FROM Families f WHERE f.isRegistered` , wordt het JSON-document met de eigenschap `isRegistered` waarin de waarde van de eigenschap gelijk is aan de JSON `true` waarde. Andere waarden (false, null en Undefined, `<number>`, `<string>`, `<object>`, `<array>`, enz.) tot het brondocument wordt uitgesloten van het resultaat leidt. 

### <a name="equality-and-comparison-operators"></a>Gelijkheid en vergelijking operatoren
De volgende tabel ziet het resultaat van gelijkheid vergelijkingen in SQL DocumentDB tussen elke twee JSON.
<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Op</strong>
         </td>
         <td valign="top">
            <strong>Niet gedefinieerd</strong>
         </td>
         <td valign="top">
            <strong>Null</strong>
         </td>
         <td valign="top">
            <strong>Boole-waarde</strong>
         </td>
         <td valign="top">
            <strong>Nummer</strong>
         </td>
         <td valign="top">
            <strong>Tekenreeks</strong>
         </td>
         <td valign="top">
            <strong>Object</strong>
         </td>
         <td valign="top">
            <strong>Matrix</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Niet gedefinieerd<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Null<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Boole-waarde<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Nummer<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Tekenreeks<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Object<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Matrix<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
      </tr>
   </tbody>
</table>

Voor andere vergelijkingsoperatoren, zoals >, > =,! =, < en < =, de volgende regels van toepassing:   

-   Vergelijking verschillende typen onbepaald resultaat.
-   Vergelijking tussen twee objecten of twee matrices wordt niet gedefinieerd.   

Als het resultaat van de scalaire expressie in het filter is niet gedefinieerd, het bijbehorende document zou niet worden opgenomen in het resultaat, aangezien onbepaald niet logisch te met "true vergelijken".

### <a name="between-keyword"></a>TUSSEN het trefwoord
U kunt ook het sleutelwoord BETWEEN gebruiken in query's voor het bereiken van waarden zoals in ANSI SQL express. TUSSEN kan worden gebruikt met tekenreeksen of getallen.

Deze query resulteert bijvoorbeeld in alle familie documenten waarin de kwaliteit van het eerste kind tussen de 1-5 (beide volledig is). 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

In tegenstelling tot in ANSI SQL kun je de tussen-component in de FROM-component zoals in het volgende voorbeeld.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Voor snellere tijden in uitvoering, moet u een indexering beleid dat gebruikmaakt van een type bereik index tegen numerieke eigenschappen/paden die worden gefilterd in de tussen-component maken. 

Het belangrijkste verschil tussen het gebruik tussen DocumentDB en ANSI SQL is dat u kunt query's bereik ten opzichte van de eigenschappen van gemengde typen express – Stel, u "grade hebt" een getal (5) in bepaalde documenten en tekenreeksen in andere ("grade4"). In deze gevallen, wordt zoals in JavaScript, een vergelijking tussen de resultaten van twee verschillende typen in een "undefined" en het document overgeslagen.

### <a name="logical-and-or-and-not-operators"></a>Logische (AND, OR en NOT) operatoren
Logische operatoren worden gebruikt voor Boole-waarden. De logische waarheid tabellen voor deze operatoren worden in de volgende tabellen weergegeven.

OR|True|False|Niet gedefinieerd
---|---|---|---
True|True|True|True
False|True|False|Niet gedefinieerd
Niet gedefinieerd|True|Niet gedefinieerd|Niet gedefinieerd

EN|True|False|Niet gedefinieerd
---|---|---|---
True|True|False|Niet gedefinieerd
False|False|False|False
Niet gedefinieerd|Niet gedefinieerd|False|Niet gedefinieerd

NIET|  |
---|---
True|False
False|True
Niet gedefinieerd|Niet gedefinieerd

### <a name="in-keyword"></a>TREFWOORD
Het sleutelwoord kan worden gebruikt om te controleren of een opgegeven waarde komt overeen met een willekeurige waarde in een lijst. Bijvoorbeeld, retourneert deze query alle documenten van de familie waar de id is een 'WakefieldFamily' of 'AndersenFamily'. 
 
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

In dit voorbeeld geeft als resultaat alle documenten waar de staat een van de opgegeven waarden.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Ternair (?) en exploitanten met Coalesce (?)
De operators Ternair en Coalesce kunnen worden gebruikt voor het maken van voorwaardelijke expressies, vergelijkbaar met populaire programmeertalen zoals C# en JavaScript. 

De operator Ternair (?) is erg handig bij het maken van nieuwe JSON-eigenschappen op elk gewenst moment. Bijvoorbeeld: nu kunt u schrijven worden gebruikt voor het classificeren van de klasse niveaus in een menselijk leesbare vorm als Beginner/Gevorderde/Geavanceerd zoals hieronder wordt weergegeven.
 
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

U kunt ook de aanroepen van de operator like in de query hieronder nesten.
 
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Als met andere query's worden als de eigenschappen waarnaar wordt verwezen in de voorwaardelijke expressie in een document ontbreekt of als de typen die worden vergeleken, verschillen, vervolgens deze documenten uitgesloten in de queryresultaten.

De operator Coalesce (?) kan worden gebruikt voor het efficiënt controleren op de aanwezigheid van een eigenschap (ook is gedefinieerd) in een document. Dit is handig bij het uitvoeren van query's tegen semi-gestructureerde of gegevens van verschillende typen. Bijvoorbeeld deze query geeft als resultaat de 'Achternaam' indien aanwezig, of de "Achternaam" Als dit niet aanwezig.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a name="quoted-property-accessor"></a>Eigenschapsaccessor genoteerde
U kunt ook toegang tot eigenschappen met behulp van de eigenschap genoteerde operator `[]`. Bijvoorbeeld, `SELECT c.grade` en `SELECT c["grade"]` zijn gelijkwaardig. Deze syntaxis is handig als u nodig hebt om een eigenschap die spaties, speciale tekens, of er gebeurt met dezelfde naam als een gereserveerd woord of een SQL-trefwoord.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a name="select-clause"></a>SELECT-component
De SELECT-component (**`SELECT <select_list>`**) is verplicht en geeft aan welke waarden worden opgehaald uit de query, op dezelfde manier als in ANSI SQL. De subset die is gefilterd op de brondocumenten worden doorgegeven naar de Projectiefase, waarbij het opgegeven JSON-waarden worden opgehaald en een nieuw JSON-object is gemaakt, voor elk doorgegeven op deze invoer. 

In het volgende voorbeeld ziet u een gewone selectiequery. 

**Query**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Geneste eigenschappen
In het volgende voorbeeld zijn er twee geneste eigenschappen projecteren `f.address.state` en `f.address.city`.

**Query**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Projectie biedt ook ondersteuning voor JSON-expressies, zoals in het volgende voorbeeld wordt getoond.

**Query**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Bekijk de rol van de `$1` hier. De `SELECT` component moet een JSON-object te maken en omdat er geen toets beschikbaar is, gebruiken we de impliciete argument variabele namen die beginnen met `$1`. Deze query resulteert bijvoorbeeld twee impliciete argumentvariabelen label `$1` en `$2`.

**Query**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Aliasing
Nu gaan we het bovenstaande voorbeeld aliasing expliciete waarden uitbreiden. Is het sleutelwoord voor aliasing gebruikt. Opmerking: optionele zoals tijdens het projecteren van de tweede waarde is `NameInfo`. 

Wanneer een query twee eigenschappen met dezelfde naam heeft, moet een of beide eigenschappen wijzigen zodat ze zijn disambiguated in het verwachte resultaat aliasing gebruiken.

**Query**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Scalaire expressies
Verwijzen naar eigenschappen ondersteunt de SELECT-component ook scalaire expressies als constanten, rekenkundige expressies, logische expressies, enz. Bijvoorbeeld, is hier een eenvoudige "Hello World"-query.

**Query**

    SELECT "Hello World"

**Resultaten**

    [{
      "$1": "Hello World"
    }]


Hier volgt een complexer voorbeeld waarin een scalaire expressie wordt gebruikt.

**Query**

    SELECT ((2 + 11 % 7)-2)/3   

**Resultaten**

    [{
      "$1": 1.33333
    }]


In het volgende voorbeeld is het resultaat van de scalaire expressie een Boole-waarde.

**Query**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f 

**Resultaten**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Object- en matrix maken
Een andere belangrijke functie van DocumentDB SQL is een array-object maken. Merk op dat we een nieuw JSON-object gemaakt in het vorige voorbeeld. Ook kunt een ook arrays maken zoals in de volgende voorbeelden.

**Query**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f 

**Resultaten**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a name="value-keyword"></a>Sleutelwoord waarde
Het sleutelwoord **waarde** biedt een manier om JSON-waarde te retourneren. De onderstaande query retourneert bijvoorbeeld de scalaire `"Hello World"` in plaats van `{$1: "Hello World"}`.

**Query**

    SELECT VALUE "Hello World"

**Resultaten**

    [
      "Hello World"
    ]


De volgende query geeft als resultaat de waarde JSON zonder de `"address"` label in de resultaten.

**Query**

    SELECT VALUE f.address
    FROM Families f 

**Resultaten**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

In het volgende voorbeeld wordt uitgebreid hierop als u wilt zien hoe JSON primitieve waarden (de leafniveau van de structuur van JSON) terug. 

**Query**

    SELECT VALUE f.address.state
    FROM Families f 

**Resultaten**

    [
      "WA",
      "NY"
    ]


###<a name="-operator"></a>* Operator
De speciale operator (*) wordt ondersteund om het document op als project-is. Wanneer gebruikt, moet de enige verwachte veld. Als u een query als `SELECT * FROM Families f` is geldig, `SELECT VALUE * FROM Families f ` en `SELECT *, f.id FROM Families f ` zijn niet geldig.

**Query**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

###<a name="top-operator"></a>TOP-operatoren
Het trefwoord TOP kan worden gebruikt om het aantal waarden in een query beperken. Als boven wordt gebruikt in combinatie met de component ORDER BY, is de resultaatset beperkt tot de eerste N aantal bestelde waarden; anders wordt de eerste N aantal resultaten in een niet-gedefinieerde volgorde. Beste, in een SELECT-instructie, gebruik altijd een ORDER BY-component met de TOP-component. Dit is de enige manier om voorspelbare geven aan welke rijen worden beïnvloed door de bovenste. 


**Query**

    SELECT TOP 1 * 
    FROM Families f 

**Resultaten**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

TOP kan worden gebruikt met een constante waarde (zoals hierboven) of met de waarde van een variabele met behulp van query's met parameters. Zie parameterquery's hieronder voor meer details.

## <a name="order-by-clause"></a>ORDER BY-component
Net als in ANSI SQL u een optionele component Order By tijdens het uitvoeren van query's opnemen kunt. De component kan bevatten een optioneel argument ASC/DESC de volgorde waarin de resultaten moeten worden opgehaald. Zie voor een meer gedetailleerde beschrijving Order By, [DocumentDB Order door scenario](documentdb-orderby.md).

Hier is bijvoorbeeld een query die families in volgorde van de verblijfplaats van de plaatsnaam wordt opgehaald.

**Query**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
    
**Resultaten**
    
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"   
      }
    ]

En Hier volgt een query opgehaald families in volgorde van aanmaakdatum, die wordt opgeslagen als een getal dat de epoch tijd, outillage, verstreken tijd sinds 1 januari 1970 in seconden.

**Query**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
    
**Resultaten**
    
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472  
      }
    ]
    
## <a name="advanced-database-concepts-and-sql-queries"></a>Geavanceerde database concepten en SQL-query 's
### <a name="iteration"></a>Iteratie
Een nieuwe constructie is toegevoegd via **het sleutelwoord in SQL DocumentDB om ondersteuning te bieden voor iteratie van JSON-matrices** . De bron van biedt ondersteuning voor herhaling. Laten we beginnen met het volgende voorbeeld:

**Query**

    SELECT * 
    FROM Families.children

**Resultaten**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Nu laten we een andere query die een iteratie uitgevoerd via de kinderen in de collectie. Let op het verschil in de resulterende matrix. In dit voorbeeld wordt gesplitst `children` en worden de resultaten in een matrix samengevoegd.  

**Query**

    SELECT * 
    FROM c IN Families.children

**Resultaten**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

Dit kan verder worden gebruikt om te filteren op elke afzonderlijke vermelding van de matrix, zoals in het volgende voorbeeld.

**Query**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Resultaten**  

    [{
      "givenName": "Lisa"
    }]

### <a name="joins"></a>Joins
In een relationele database is het nodig deel te nemen aan verschillende tabellen heel belangrijk. Het is het logische gevolg van genormaliseerde schema's ontwerpen. Met dit behandelt DocumentDB de model Gedenormaliseerde gegevens van schema-vrije documenten. Dit is het logische equivalent van een "self join".

De syntaxis van de taal ondersteunt is < from_source1 > < from_source2 > JOIN JOIN... JOIN < from_sourceN >. Hiermee wordt over het algemeen een set van **N**- tupels (tupel met **N** waarden). Elke tupel is geproduceerd door alle collectie aliassen met iteratie van hun respectieve sets waarden. Met andere woorden, is dit een volledig cross-product van de sets die deel uitmaken van de join.

De volgende voorbeelden wordt de werking van de JOIN-component. In het volgende voorbeeld is het resultaat leeg sinds het kruisproduct van elk document van de bron en een lege verzameling is leeg.

**Query**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Resultaten**  

    [{
    }]


In het volgende voorbeeld wordt de join is tussen de hoofdmap van het document en de `children` onderliggende root. Het is een cross-product tussen twee JSON-objecten. Het feit dat kinderen een matrix is niet effectief in de JOIN omdat we werkt met één hoofd is de matrix voor kinderen. Het resultaat bevat dus slechts twee resultaten, omdat het kruisproduct van elk document met de array precies één document levert.

**Query**

    SELECT f.id
    FROM Families f
    JOIN f.children
 
**Resultaten**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


In het volgende voorbeeld ziet u een meer conventionele join:

**Query**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Resultaten**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



Allereerst om te weten is dat de `from_source` van de **JOIN** -component is een iterator. Dus is de stroom in dit geval als volgt:  

-   Vouw elk onderliggend element **c** in de matrix.
-   Een cross-product met de hoofdmap van het document **f** met elk onderliggend element in de eerste stap is afgevlakt **c** van toepassing.
-   Ten slotte, de basis- **f** de eigenschap name object alleen in project. 

Het eerste document (`AndersenFamily`) bevat slechts één onderliggend element, zodat de resultaatset bevat slechts een enkel object dat overeenkomt met dit document. Het tweede document (`WakefieldFamily`) bevat twee kinderen. Het kruisproduct levert dus een afzonderlijk object voor elk kind, waardoor het aldus twee objecten, één voor elk kind dat overeenkomt met dit document. Houd er rekening mee dat de hoofdmap velden in deze beide documenten dezelfde zijn, net zoals u in een cross-product verwachten zou.

Formulier tupels is van het cross-product in een vorm die anders moeilijk project het echte nut van de JOIN. Zoals we in het volgende voorbeeld zien, kan u filteren op de combinatie van een tupel die kunt de gebruiker hebt gekozen een voorwaarde is voldaan door de tupels algemene.

**Query**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
 
**Resultaten**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



In dit voorbeeld wordt een natuurlijke uitbreiding van het vorige voorbeeld en een dubbele join uitvoert. Het kruisproduct kan dus worden weergegeven als de volgende pseudo code.

    for-each(Family f in Families)
    {   
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`een kind een huisdier heeft is. Dus het kruisproduct dat resulteert in een rij (1*1*1) van deze familie. WakefieldFamily echter twee kinderen heeft, maar slechts één onderliggende 'Jesse' huisdieren. Jesse heeft maar 2 huisdieren. Het kruisproduct levert dus 1 op*1*, 2 = 2 rijen uit deze familie.

In het volgende voorbeeld, er is een extra filter op `pet`. Dit omvat niet alle tupels waar de naam huisdier niet is "Schaduw". U ziet dat we kunnen bouwen tupels van matrices, filter op een van de elementen van de tupel, en een combinatie van de elementen van project. 

**Query**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Resultaten**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a name="javascript-integration"></a>JavaScript-integratie
DocumentDB biedt een programmeermodel voor het uitvoeren van JavaScript op basis van toepassingslogica rechtstreeks op de collecties van opgeslagen procedures en triggers. Hierdoor kunnen beide:

-   Mogelijkheid tot krachtige transactionele CRUD bewerkingen en query's voor documenten uit een collectie op grond van de naadloze integratie van JavaScript runtime rechtstreeks in de database-engine. 
-   Een natuurlijke modellering van besturing, variabele scoping, toewijzing en integratie van primitieven met databasetransacties afhandeling van uitzonderingen. Raadpleeg de documentatie JavaScript server-side programmeren voor meer informatie over ondersteuning voor JavaScript integratie DocumentDB.

###<a name="user-defined-functions-udfs"></a>Door de gebruiker gedefinieerde functies (UDF's)
DocumentDB SQL biedt samen met de typen al gedefinieerd in dit artikel, ondersteuning voor gebruiker gedefinieerde functies (UDF). Scalaire UDF's worden in het bijzonder waar de ontwikkelaars kunnen nul of meer argumenten doorgeven en resultaat een enkel argument terug ondersteund. Elk van deze argumenten zijn voor juridische JSON-waarden wordt gecontroleerd.  

De DocumentDB SQL-syntaxis is uitgebreid met ondersteuning van aangepaste toepassingslogica met behulp van deze gebruiker gedefinieerde functies. UDF's met DocumentDB kunnen worden geregistreerd en vervolgens worden verwezen als onderdeel van een SQL-query. De UDF's zijn in feite exquisitely ontworpen om te worden aangeroepen door query's. UDF's doen als gevolg van deze keuze geen toegang tot het contextobject met de andere JavaScript typen (opgeslagen procedures en triggers). Omdat de query's uitvoeren als alleen-lezen, zij de primaire of secundaire replica's kunnen uitvoeren. Daarom zijn UDF's ontworpen voor secundaire replica's, in tegenstelling tot andere typen JavaScript.

Hieronder ziet u een voorbeeld van hoe een UDF op de DocumentDB-database onder een documentverzameling specifiek kan worden geregistreerd.

   
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };
       
       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
                                                                             
Het voorgaande voorbeeld maakt een UDF waarvan de naam is `REGEX_MATCH`. Twee JSON string-waarden accepteert `input` en `pattern` en controles als de eerste komt overeen met het patroon wordt opgegeven in de tweede van JavaScript string.match() functie te gebruiken.


Nu kunnen we deze UDF gebruiken in een query in een projectie. UDF's moeten worden gekwalificeerd met de hoofdlettergevoelige voorvoegsel "udf". Wanneer aangeroepen vanuit query's. 

>[AZURE.NOTE] Vóór 17-3/2015 ondersteund DocumentDB UDF aanroepen zonder de "udf". het voorvoegsel dat als REGEX_MATCH() selecteren. Dit patroon aanroepende is afgeschaft.  

**Query**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Resultaten**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

De UDF kan ook worden gebruikt in een filter, zoals in het volgende voorbeeld zijn ook gekwalificeerd met de "udf". voorvoegsel:

**Query**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Resultaten**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


In wezen UDF's zijn geldig scalaire expressies en kunnen worden gebruikt in projecties en filters. 

Vouw op de kracht van UDF's, we kijken naar een ander voorbeeld met voorwaardelijke logica:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                switch (city) {
                    case 'seattle':
                        return 520;
                    case 'NY':
                        return 410;
                    case 'Chicago':
                        return 673;
                    default:
                        return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
    
    
Hieronder wordt een voorbeeld dat gebruik maakt van de UDF.

**Query**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f 

**Resultaten**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Zoals in de voorgaande voorbeelden presenteren, integreren UDF's in de kracht van de taal JavaScript biedt een rijke programmeerbare interface hiervoor complexe procedures, voorwaardelijke logica met behulp van de ingebouwde mogelijkheden van JavaScript runtime van de SQL-DocumentDB.

DocumentDB SQL biedt de argumenten voor de UDF's voor elk document in de bron in het huidige stadium (WHERE-component of SELECT-component) van de UDF worden verwerkt. Het resultaat wordt opgenomen in de algemene pijplijn naadloos. Als de eigenschappen waarnaar door de UDF parameters zijn niet beschikbaar in de JSON-waarde, beschouwd als de parameter niet gedefinieerd en dus het oproepen van de UDF volledig wordt overgeslagen. Op dezelfde manier als het resultaat van de UDF gedefinieerd is, deze niet opgenomen in het resultaat. 

Kortom zijn UDF's geweldige middelen om complexe bedrijfslogica als onderdeel van de query.

### <a name="operator-evaluation"></a>Evaluatie van de operator
DocumentDB, tekent door het feit dat een database JSON werkt hetzelfde als met JavaScript-operatoren en de semantiek van de evaluatie. Terwijl DocumentDB probeert te behouden semantiek JavaScript in JSON ondersteuning, wordt de evaluatie van de bewerking afwijkt in sommige gevallen.

In SQL DocumentDB, zijn in tegenstelling tot traditionele SQL welke typen waarden vaak niet bekend totdat de waarden daadwerkelijk worden opgehaald uit de database. Om efficiënt uitvoeren van query's, hebben de meeste van de operatoren strikte eisen. 

DocumentDB SQL uitvoeren niet impliciete omzettingen, in tegenstelling tot JavaScript. Bijvoorbeeld, een query, zoals `SELECT * FROM Person p WHERE p.Age = 21` komt overeen met documenten waarin een leeftijd eigenschap waarvan de waarde 21 is. Andere documenten waarvan de eigenschap leeftijd komt overeen met de tekenreeks "21" of andere mogelijk oneindige variaties, zoals "021", "21,0", "0021", "00021", enz., zal niet worden afgestemd. Dit is in tegenstelling tot de JavaScript waarin de string-waarden impliciet omgezet naar getallen zijn (op basis van de operator, ex: ==). Deze keuze is cruciaal voor het efficiënte index zoeken in SQL DocumentDB. 

## <a name="parameterized-sql-queries"></a>SQL-query's met parameters
DocumentDB biedt ondersteuning voor query's met parameters die worden uitgedrukt met de vertrouwde @ notatie. Geparametriseerde SQL biedt robuuste verwerken en het ontsnappen van gebruikersinvoer, accidentele blootstelling van gegevens via SQL injection te voorkomen. 

U kunt bijvoorbeeld een query schrijven waarmee de achternaam en de provincie als parameters krijgt en voeren voor de verschillende waarden van de laatste naam en adres staat op basis van gebruikersinvoer.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Deze aanvraag kan vervolgens worden verzonden naar DocumentDB als een query met parameters JSON zoals hieronder weergegeven.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Het argument naar boven kan worden ingesteld met behulp van query's met parameters, zoals hieronder wordt weergegeven.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Parameterwaarden kunnen bestaan uit elke geldige JSON (tekenreeksen, cijfers, Boole-waarden, null, zelfs arrays of JSON genest). Ook omdat DocumentDB schema kleiner, worden parameters niet gevalideerd met elk type.

##<a name="built-in-functions"></a>Ingebouwde functies
DocumentDB ondersteunt ook een aantal ingebouwde functies voor veelgebruikte bewerkingen, die kunnen worden gebruikt in query's als de gebruiker gedefinieerde functies (UDF's).

<table>
<tr>
<td>Wiskundige functies</td> 
<td>ABS, plafond, EXP, FLOOR, logboek, LOG10, energie, ronde, teken, WORTEL, VIERKANT, geheel, ACOS, ASIN, BOOGTAN, ATN2, COS, COT, graden PI, radialen, SIN en TAN</td>
</tr>
<tr>
<td>Controle van de functies van het type</td>    
<td>IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED en IS_PRIMITIVE</td>
</tr>
<tr>
<td>Tekenreeksfuncties</td>   
<td>CONCAT bevat, ENDSWITH, INDEX_OF, links, lengte, lager, LTRIM, vervangen, REPLICEREN, REVERSE, rechts, RTRIM, STARTSWITH, SUBTEKENREEKS en hoofdletters</td>
</tr>
<tr>
<td>Array functies</td>    
<td>ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH en ARRAY_SLICE</td>
</tr>
<tr>
<td>Ruimtelijke functies</td>  
<td>ST_DISTANCE, ST_WITHIN, ST_ISVALID en ST_ISVALIDDETAILED</td>
</tr>
</table>  

Als u een gebruiker gedefinieerde functie (UDF) die een ingebouwde functie nu beschikbaar is op dit moment gebruikt, moet u de bijbehorende ingebouwde functie zoals is het verstandig om uit te voeren sneller en efficiënter. 

### <a name="mathematical-functions"></a>Wiskundige functies
Wiskundige functies elke uitvoeren van een berekening, meestal op basis van invoerwaarden die als argumenten worden geleverd en een numerieke waarde als resultaat. Hier volgt een lijst met ondersteunde ingebouwde wiskundige functies.

<table>
<tr>
<td><strong>Gebruik</strong></td>
<td><strong>Beschrijving</strong></td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_abs">ABS (num_expr)</a></td> 
<td>Geeft als resultaat de absolute (positieve) waarde van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ceiling">PLAFOND (num_expr)</a></td> 
<td>Deze eigenschap retourneert het kleinste gehele getal groter dan of gelijk is aan de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_floor">FLOOR (num_expr)</a></td> 
<td>Deze eigenschap retourneert het grootste gehele getal kleiner dan of gelijk aan de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_exp">EXP (num_expr)</a></td> 
<td>Geeft als resultaat de exponent van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log">LOGBOEK (num_expr [, base])</a></td> 
<td>Berekent de natuurlijke logaritme van de opgegeven numerieke expressie of de logaritme met het opgegeven grondtal.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log10">LOG10 (num_expr)</a></td> 
<td>Geeft als resultaat de logaritmische waarde van grondtal 10 van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_round">ROUND (num_expr)</a></td> 
<td>Geeft als resultaat een numerieke waarde, afgerond op het dichtstbijzijnde gehele getal.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_trunc">TRUNC (num_expr)</a></td> 
<td>Geeft als resultaat een numerieke waarde, afgekapt tot het dichtstbijzijnde gehele getal.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sqrt">SQRT (num_expr)</a></td>   
<td>Berekent de vierkantswortel van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_square">VIERKANT (num_expr)</a></td>   
<td>Geeft als resultaat het kwadraat van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_power">ENERGIE (num_expr, num_expr)</a></td>   
<td>De kracht van de opgegeven numerieke expressie retourneert naar de opgegeven waarde.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sign">TEKEN (num_expr)</a></td>   
<td>Retourneert de waarde teken (-1, 0, 1) van de opgegeven numerieke expressie.</td>
</tr>
<tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_acos">ACOS (num_expr)</a></td>   
<td>Geeft als resultaat de hoek in radialen, waarvan de cosinus de opgegeven numerieke expressie is. ook de boogcosinus genoemd.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_asin">ASIN (num_expr)</a></td>   
<td>Geeft als resultaat de hoek in radialen, waarvan de sinus de opgegeven numerieke expressie is. Dit is ook de boogsinus genoemd.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atan">ATAN (num_expr)</a></td>   
<td>Geeft als resultaat de hoek in radialen, waarvan de tangens de opgegeven numerieke expressie is. Dit is ook de boogtangens genoemd.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atn2">ATN2 (num_expr)</a></td>   
<td>Geeft als resultaat de hoek in radialen tussen de positieve x-as en de ray van de oorsprong naar het punt (y, x), waarbij x en y zijn de waarden van de twee opgegeven float-expressies.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cos">COS (num_expr)</a></td> 
<td>Geeft als resultaat de trigonometrische cosinus van de opgegeven hoek in radialen, in de opgegeven expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cot">COT (num_expr)</a></td> 
<td>Geeft als resultaat de trigonometrische cotangens van de opgegeven hoek in radialen, in de opgegeven numerieke expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_degrees">GRADEN (num_expr)</a></td> 
<td>Geeft de overeenkomstige hoek in graden voor een hoek in radialen.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_pi">PI)</a></td>   
<td>Geeft als resultaat de constante waarde van PI.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_radians">RADIALEN (num_expr)</a></td> 
<td>Radialen geretourneerd als een numerieke expressie, in graden worden ingevoerd.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sin">SIN (num_expr)</a></td> 
<td>Geeft als resultaat de trigonometrische sinus van de opgegeven hoek in radialen, in de opgegeven expressie.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_tan">TAN (num_expr)</a></td> 
<td>Berekent de tangens van de expressie invoeren in de opgegeven expressie.</td>
</tr>

</table> 

U kunt nu bijvoorbeeld query's als volgt uitvoeren:

**Query**

    SELECT VALUE ABS(-4)

**Resultaten**

    [4]

Het belangrijkste verschil tussen de DocumentDB functies in vergelijking met ANSI SQL is dat ze goed samenwerken met schema kleiner en gemengde schemagegevens zijn ontworpen. Als u een document waarin de eigenschap Size ontbreekt of is een niet-numerieke waarde, zoals 'Onbekend' wordt het document in plaats van een fout wordt geretourneerd, wordt overgeslagen.

### <a name="type-checking-functions"></a>Controle van de functies van het type
Het type controle-functies kunnen u het type van een expressie in een SQL-query's te controleren. Type controle functies kunnen worden gebruikt om te bepalen het type van eigenschappen in-documenten als variabele of onbekend is. Hier volgt een lijst met ondersteunde ingebouwde typecontrole functies.

<table>
<tr>
  <td><strong>Gebruik</strong></td>
  <td><strong>Beschrijving</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (expr)</a></td>
  <td>Deze eigenschap retourneert een Boolean-waarde die aangeeft of het type van de waarde een matrix is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (expr)</a></td>
  <td>Deze eigenschap retourneert een Boolean-waarde die aangeeft of het type van de waarde een Boolean-waarde is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (expr)</a></td>
  <td>Deze eigenschap retourneert een Boolean-waarde die aangeeft of het type van de waarde null is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (expr)</a></td>
  <td>Deze eigenschap retourneert een Boolean-waarde die aangeeft of het type van de waarde een getal is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (expr)</a></td>
  <td>Deze eigenschap retourneert een Boolean-waarde die aangeeft of het type van de waarde een JSON-object is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (expr)</a></td>
  <td>Deze eigenschap retourneert een Boolean-waarde die aangeeft of het type van de waarde een tekenreeks is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (expr)</a></td>
  <td>Deze eigenschap retourneert een Boolean die aangeeft als de eigenschap een waarde is toegewezen.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (expr)</a></td>
  <td>Deze eigenschap retourneert een Boolean-waarde die aangeeft of het type van de waarde een tekenreeks, getal, Boole-waarde of null is.</td>
</tr>

</table>

Met deze functies, kunt u nu uitvoeren query's als volgt uit:

**Query**

    SELECT VALUE IS_NUMBER(-4)

**Resultaten**

    [true]

### <a name="string-functions"></a>Tekenreeksfuncties
De volgende scalaire functies een bewerking uitvoeren op een invoerwaarde tekenreeks en retourneert een tekenreeks, numerieke of Booleaanse waarde. Hier wordt een lijst met ingebouwde tekenreeks-functies:

Gebruik|Beschrijving
---|---
[LENGTE (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length)|Geeft als resultaat het aantal tekens van de opgegeven tekenreeksexpressie
[SAMENVOEGEN (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)|Retourneert een tekenreeks die het resultaat van twee of meer tekenreeksen aaneen.
[SUBTEKENREEKS (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring)|Geeft als resultaat een deel van de tekenreeksexpressie.
[STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith)|Deze eigenschap retourneert een Boolean die aangeeft of de eerste expressie een tekenreeks eindigt met de tweede
[ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith)|Deze eigenschap retourneert een Boolean die aangeeft of de eerste expressie een tekenreeks eindigt met de tweede
[BEVAT (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains)|Deze eigenschap retourneert een Boolean die aangeeft of de eerste expressie een tekenreeks bevat de tweede.
[INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of)|Deze eigenschap retourneert de beginpositie van het eerste exemplaar van de tweede tekenreeks expressie binnen de eerste expressie in de opgegeven tekenreeks of -1 als de tekenreeks niet wordt gevonden.
[LINKS (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left)|Deze eigenschap retourneert het linker gedeelte van een tekenreeks met het opgegeven aantal tekens.
[RECHTS (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right)|Deze eigenschap retourneert het rechterdeel van een tekenreeks met het opgegeven aantal tekens.
[LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim)|Deze eigenschap retourneert een tekenreeksexpressie na het verwijderen van toonaangevende lege cellen.
[RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim)|Geeft als resultaat een tekenreeksexpressie na alle volgspaties worden afgekapt.
[LAGER (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower)|Deze eigenschap retourneert een tekenreeksexpressie na hoofdletters Tekengegevens converteren naar kleine letters.
[HOOFDLETTERS (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper)|Deze eigenschap retourneert een tekenreeksexpressie na kleine Tekengegevens converteren naar hoofdletters.
[VERVANGEN (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace)|Alle exemplaren van een opgegeven tekenreekswaarde vervangen door een andere tekenreeks.
[REPLICEREN (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replicate)|Herhaalt een string-waarde voor een opgegeven aantal keren.
[OMKEREN (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse)|De omgekeerde volgorde van een string-waarde geretourneerd.

U kunt nu een query's de volgende uitvoeren met behulp van deze functies. Bijvoorbeeld: u kunt terugkeren de familienaam in hoofdletters als volgt:

**Query**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Resultaten**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Of het samenvoegen van strings zoals in het volgende voorbeeld:

**Query**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Resultaten**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Tekenreeks-functies kunnen ook worden gebruikt in de WHERE-component om resultaten te filteren, zoals in het volgende voorbeeld:

**Query**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Resultaten**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Array functies
De volgende scalaire functies een bewerking uitvoeren op een invoerwaarde matrix en numerieke return, Boole-waarde of een matrix. Hier is een tabel van een van ingebouwde matrixfuncties:

Gebruik|Beschrijving
---|---
[ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length)|Deze eigenschap retourneert het aantal elementen van de expressie van de opgegeven matrix.
[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)|Geeft als resultaat een matrix die het resultaat is van het samenvoegen van twee of meer matrixwaarden.
[ARRAY_CONTAINS (arr_expr, expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)|Deze eigenschap retourneert een Boolean-waarde die aangeeft of de matrix de opgegeven waarde bevat.
[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)|Geeft als resultaat een deel van de expressie van een matrix.

Array functies kunnen worden gebruikt voor het bewerken van matrices in JSON. Hier is bijvoorbeeld een query geeft als resultaat alle documenten waarin een van de ouders "Robin Wakefield". 

**Query**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Resultaten**

    [{
      "id": "WakefieldFamily"
    }]

Hier is een ander voorbeeld dat ARRAY_LENGTH gebruikt om het aantal kinderen per gezin.

**Query**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Resultaten**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Ruimtelijke functies

DocumentDB ondersteunt de volgende ingebouwde functies voor Open georuimtelijke Consortium (OGC) voor georuimtelijke opvragen. Voor meer informatie over georuimtelijke ondersteuning in DocumentDB, raadpleegt u [werken met georuimtelijke gegevens in Azure DocumentDB](documentdb-geospatial.md). 

<table>
<tr>
  <td><strong>Gebruik</strong></td>
  <td><strong>Beschrijving</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Retourneert de afstand tussen de twee GeoJSON point-expressies.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Deze eigenschap retourneert een Boole-expressie die aangeeft of het GeoJSON punt opgegeven in het eerste argument binnen de GeoJSON veelhoek in het tweede argument is.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Deze eigenschap retourneert een Boolean-waarde die aangeeft of de opgegeven GeoJSON punt of veelhoek expressie ongeldig is.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Deze eigenschap retourneert een JSON-waarde die een Booleaanse waarde als de opgegeven GeoJSON punt of veelhoek expressie geldig is en als ongeldig, ook de reden als een string-waarde.</td>
</tr>
</table>

Ruimtelijke functies kunnen worden gebruikt voor het uitvoeren van querries dicht tegen de ruimtelijke gegevens. Hier is bijvoorbeeld een query die alle familie documenten die binnen 30 km van de opgegeven locatie met behulp van de ingebouwde functie voor ST_DISTANCE. 

**Query**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Resultaten**

    [{
      "id": "WakefieldFamily"
    }]

Als u ruimtelijke indexeren in uw beleid indexing, worden vervolgens 'afstand query's ' geserveerd efficiënt via de index. Zie de sectie hieronder voor meer informatie over ruimtelijke indexeren. Als u niet een ruimtelijke index voor de opgegeven paden hebt, kunt u wel ruimtelijke query's uitvoeren door te geven `x-ms-documentdb-query-enable-scan` met de waarde ingesteld op 'true' verzoek-header. In .NET, kan dit worden gedaan door het optionele argument voor **FeedOptions** op query's met [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) ingesteld op true. 

ST_WITHIN kan worden gebruikt om te controleren als een punt binnen een veelhoek ligt. Veelhoeken worden meestal gebruikt om de grenzen zoals postcodes, staat de grenzen of natuurlijke formaties vertegenwoordigen. Opnieuw als u ruimtelijke indexeren in uw beleid indexing, vervolgens 'binnen' query's worden geserveerd efficiënt via de index. 

Veelhoek argumenten in ST_WITHIN slechts één keer kunnen bevatten, dat wil zeggen de veelhoeken mag geen gaten erin. Controleer de [DocumentDB grenzen](documentdb-limits.md) voor het maximum aantal punten in een veelhoek is toegestaan voor een ST_WITHIN-query.

**Query**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Resultaten**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Vergelijkbaar met hoe niet-overeenkomende typen works in DocumentDB-query als de locatiewaarde die is opgegeven in een argument onjuist of ongeldig is, wordt het resultaat **niet gedefinieerd** en het document evalueren van de resultaten van de query worden overgeslagen. Als uw zoekopdracht geen resultaten oplevert, uitvoering ST_ISVALIDDETAILED voor foutopsporing waarom het type spatail is ongeldig.     

ST_ISVALID en ST_ISVALIDDETAILED kunnen worden gebruikt om te controleren of een ruimtelijk object geldig is. Bijvoorbeeld controleert de volgende query de geldigheid van een punt met een buiten bereik latitude-waarde (-132.8). ST_ISVALID alleen een Boolean-waarde retourneert en ST_ISVALIDDETAILED retourneert de Boolean-waarde en een tekenreeks met de reden waarom het wordt beschouwd als ongeldig.

**Query**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Resultaten**

    [{
      "$1": false
    }]

Deze functies kunnen ook worden gebruikt voor het valideren van veelhoeken. Bijvoorbeeld, gebruikt ST_ISVALIDDETAILED voor het valideren van een veelhoek die niet wordt afgesloten. 

**Query**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Resultaten**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
Die loopt van ruimtelijke functies en de SQL-syntaxis voor DocumentDB. Nu eens kijken hoe LINQ query werkt en hoe deze samenwerkt met de syntaxis we eerder aan bod kwamen.

## <a name="linq-to-documentdb-sql"></a>LINQ aan SQL DocumentDB
LINQ is een .NET-programmeermodel waarin de berekening als query's op streams van objecten. DocumentDB biedt een client side library interface met LINQ doordat een conversie tussen JSON en .NET-objecten en een toewijzing van een subset van LINQ query query's DocumentDB. 

De volgende afbeelding ziet de architectuur van het LINQ-query's met behulp van DocumentDB te ondersteunen.  Met de DocumentDB-client, kunnen ontwikkelaars een **IQueryable** -object dat de DocumentDB query provider, vervolgens het LINQ-query in een query DocumentDB zet rechtstreeks een query maken. De query wordt vervolgens doorgegeven aan de server DocumentDB voor het ophalen van een set resultaten in JSON-indeling. De resultaten worden gedeserialiseerd in een stroom van .NET-objecten op de client.

![Architectuur van ondersteuning voor LINQ query's met DocumentDB - SQL-syntaxis, JSON query language, basisbegrippen in database en SQL-query 's][1]
 


### <a name="net-and-json-mapping"></a>.NET en JSON-toewijzing
De toewijzing tussen .NET-objecten en JSON-documenten is natuurlijk - elk gegevensveld lid is toegewezen aan een JSON-object, waarbij de naam van het veld wordt toegewezen aan de 'sleutel' deel van het object en het deel "waarde" is toegewezen aan de waarde van het object recursief. Bekijk het volgende voorbeeld. De familie-object gemaakt wordt toegewezen aan de JSON-document, zoals hieronder wordt weergegeven. En omgekeerd, de JSON-document is toegewezen aan een .NET-object.

**C#-klasse**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };
    
    public struct Parent
    {
        public string familyName;
        public string givenName;
    };
    
    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };
    
    public class Pet
    {
        public string givenName;
    };
    
    public class Address
    {
        public string state;
        public string county;
        public string city;
    };
    
    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>LINQ aan SQL-vertaling
De query DocumentDB provider voert een beste inspanning toewijzing van een LINQ query in een DocumentDB SQL-query. In de onderstaande beschrijving, nemen we aan dat de lezer is een elementaire kennis van LINQ.

Voor het typesysteem ondersteunen we eerst alle JSON primitieve typen – numerieke typen, boolean, string en een null. Alleen deze JSON-typen worden ondersteund. De volgende scalaire expressies worden ondersteund.

-   Constante waarden – deze constante waarden van de primitieve gegevenstypen bevat op het moment dat de query wordt geëvalueerd.

-   Matrix/indexexpressies – deze expressies verwijzen naar de eigenschap van een object of een array-element.

        family.Id;
        family.children[0].familyName;
        family.children[0].grade;
        family.children[n].grade; //n is an int variable

-   Rekenkundige expressies - deze veelgebruikte rekenkundige expressies op numerieke en Boole-waarden bevatten. Raadpleeg de SQL-specificatie voor de volledige lijst.

        2 * family.children[0].grade;
        x + y;

-   Tekenreeksexpressie vergelijking - hierbij een string-waarde met een constant string-waarde te vergelijken.  
 
        mother.familyName == "Smith";
        child.givenName == s; //s is a string variable

-   Object/matrix expressie maken - deze retour expressies een object van het waardetype van de samengestelde of anonieme of een array van objecten. Deze waarden kunnen worden genest.

        new Parent { familyName = "Smith", givenName = "Joe" };
        new { first = 1, second = 2 }; //an anonymous type with 2 fields              
        new int[] { 3, child.grade, 5 };

### <a name="list-of-supported-linq-operators"></a>Lijst met ondersteunde operators voor LINQ
Hier volgt een lijst van ondersteunde LINQ operatoren in de LINQ provider met de DocumentDB .NET SDK is opgenomen.

-   **Selecteer**: projecties vertalen naar de SQL SELECT-met inbegrip van de bouw van object
-   **Waar**: vertalen naar SQL WHERE Filters en ondersteuning voor de conversie van & &, || en! met de SQL-operatoren
-   **SelectMany**: kan de afwikkeling van arrays op de SQL JOIN-component. Kan worden gebruikt om expressies te filteren op matrixelementen keten/nest
-   **Sorteren op- en OrderByDescending**: wordt omgezet in een ORDER BY Oplopend/Aflopend sorteren:
-   **CompareTo**: vertaald naar bereik vergelijkingen. Meestal gebruikt voor tekenreeksen, aangezien die niet vergelijkbaar zijn in .NET
-   **Nemen**: vertaalt naar boven SQL voor het beperken van de resultaten van een query
-   **Wiskundige functies**: vertaling van ondersteunt. NET van Abs, Acos, Asin, BOOGTAN, maximum, Cos, Exp, vloer, logboek, Log10, Pow, ronde, teken, Sin, WORTEL, Tan, Truncate naar de equivalente SQL-ingebouwde functies.
-   **Tekenreeks-functies**: vertaling van ondersteunt. NET van Concat, bevat, EndsWith, IndexOf, aantal, ToLower, TrimStart, vervangen, Reverse, TrimEnd, StartsWith, subtekenreeks, ToUpper naar de equivalente SQL-ingebouwde functies.
-   **Array functies**: vertaling van ondersteunt. De netto Concat bevat en telt de equivalente SQL-ingebouwde functies.
-   **Georuimtelijke Extensiefuncties**: vertaling van stub methoden afstand binnen IsValid en IsValidDetailed naar de equivalente SQL-ingebouwde functies ondersteund.
-   **De gebruiker gedefinieerde functie extensie functie**: ondersteunt vertaling van de stub-methode UserDefinedFunctionProvider.Invoke naar de bijbehorende gebruiker gedefinieerde functie.
-   **Diversen**: vertaling van de voorwaardelijke operators en coalesce ondersteunt. Bevat tekenreeks bevat, ARRAY_CONTAINS of de SQL IN afhankelijk van de context kan worden vertaald.

### <a name="sql-query-operators"></a>SQL-query 's
Hier volgen enkele voorbeelden die illustreren hoe sommige van de standaardoperators voor query's LINQ vertaald naar DocumentDB-query's.

#### <a name="select-operator"></a>Operator selecteren
De syntaxis is `input.Select(x => f(x))`, waarbij `f` is een scalaire expressie.

**LINQ lambda-expressie**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda-expressie**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda-expressie**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany operator
De syntaxis is `input.SelectMany(x => f(x))`, waarbij `f` is een scalaire expressie met als een verzamelingtype retourwaarde.

**LINQ lambda-expressie**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Waar operator
De syntaxis is `input.Where(x => f(x))`, waarbij `f` is een scalaire expressie die een Boolean-waarde als resultaat gegeven.

**LINQ lambda-expressie**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda-expressie**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Samengestelde SQL-query 's
De bovenstaande operators kunnen worden samengesteld uit om krachtige query's. Aangezien DocumentDB geneste verzamelingen ondersteunt, kan de samenstelling worden samengevoegd of genest.

#### <a name="concatenation"></a>Samenvoeging 

De syntaxis is `input(.|.SelectMany())(.Select()|.Where())*`. Een aaneengeschakelde query kunt starten met een optioneel `SelectMany` query gevolgd door meerdere `Select` of `Where` operatoren.


**LINQ lambda-expressie**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda-expressie**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda-expressie**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
            
**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda-expressie**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Nesten

De syntaxis is `input.SelectMany(x=>x.Q())` waar Q is een `Select`, `SelectMany`, of `Where` operator.

In een geneste query, wordt de inner query toegepast op elk element van de buitenste collectie. Een belangrijke functie is dat de inner query naar de velden van de elementen in de buitenste collectie zoals verwijzen kan joins self.

**LINQ lambda-expressie**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda-expressie**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));
            
**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda-expressie**
            
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a name="executing-sql-queries"></a>SQL-query's uitvoeren
DocumentDB beschrijft bronnen via een REST API die kan worden aangeroepen door een HTTP/HTTPS-verzoeken staat taal. Daarnaast biedt DocumentDB voor diverse populaire talen, zoals .NET, Node.js, JavaScript en Python programming-bibliotheken. De REST API en de verschillende bibliotheken ondersteuning via SQL query. De .NET SDK biedt ondersteuning voor LINQ naast SQL query.

De volgende voorbeelden laten zien hoe een query maken en indienen tegen een database DocumentDB account.

### <a name="rest-api"></a>REST-API
DocumentDB biedt een open RESTful programmeermodel via HTTP. Rekeningen van de database kunnen worden ingericht met een abonnement op Azure. Het model met DocumentDB bestaat uit een set van middelen onder de databaseaccount van een die kan worden geadresseerd met een logische en stabiele URI. Een set bronnen een feed in dit document genoemd. Een databaseaccount bestaat uit een verzameling databases, meerdere collecties, die elk van die afslag documenten, UDF's en andere brontypen bevatten.

Het model basic interactie met deze bronnen is via de HTTP-woorden GET, PUT, POST en verwijderen met de standaard interpretatie daarvan. De POST-bewerking wordt gebruikt voor het maken van een nieuwe resource, voor het uitvoeren van een opgeslagen procedure of voor de afgifte van een query voor DocumentDB. Query's zijn altijd alleen bewerkingen met geen bijwerkingen lezen.

De volgende voorbeelden ziet een advertentie voor een DocumentDB query ten opzichte van een collectie met de twee voorbeelddocumenten dat we tot nu toe hebt bekeken. De query bevat een eenvoudig filter op de eigenschap JSON-naam. Let op het gebruik van de `x-ms-documentdb-isquery` en Content-Type: `application/query+json` kop om aan te geven dat de bewerking een query is.


**Aanvraag**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }
    

**Resultaten**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Het tweede voorbeeld ziet u een complexere query die meerdere resultaten van de join retourneert.

**Aanvraag**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json
    
    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Resultaten**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Als de resultaten van een query niet past binnen een enkele pagina van de resultaten, de REST API en geeft vervolgens een voortzetting token via de `x-ms-continuation-token` response-header. Clients kunnen resultaten door de koptekst in de volgende resultaten pagineren. Het aantal resultaten per pagina kan ook worden beheerd via de `x-ms-max-item-count` aantal kop.

Voor het beheer van het beleid van de consistentie van gegevens voor query's gebruiken de `x-ms-consistency-level` kop, net als alle aanvragen voor REST-API. Voor de consistentie van de sessie wordt geëist dat ook de meest recente echo `x-ms-session-token` Cookie-header in het query-verzoek. Houd er rekening mee dat de opgevraagde collectie indexing beleid kan ook van invloed zijn op de consistentie van de resultaten van query. Met de standaard instellingen voor indexeren voor verzamelingen de index is altijd up-to-date met de inhoud van het document en queryresultaten komt overeen met de gekozen voor data consistentie. Als het beleid indexing is versoepeld voor Lazy, kunnen query's verlopen resultaten geretourneerd. Raadpleeg voor meer informatie [DocumentDB consistentie niveaus] [consistency-levels].

Als de geconfigureerde indexing beleid op de collectie kan niet ondersteund door de opgegeven query, retourneert de server DocumentDB 400 'ongeldig verzoek". Deze informatie wordt teruggestuurd voor query's bereik ten opzichte van de paden die zijn geconfigureerd voor zoekacties hash (gelijkheid) en paden die expliciet worden uitgesloten van indexering. De `x-ms-documentdb-query-enable-scan` header kan worden opgegeven om de query te scannen wanneer een index niet beschikbaar is.

### <a name="c-net-sdk"></a>C# (.NET) SDK
De .NET SDK biedt ondersteuning voor LINQ- en SQL query. In het volgende voorbeeld ziet u hoe de eenvoudige filterquery die eerder in dit document uitvoeren.


    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


In dit voorbeeld worden twee eigenschappen van gelijkheid binnen elk document vergeleken en anonieme projecties gebruikt. 


    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


In het volgende voorbeeld ziet u joins, uitgedrukt via LINQ SelectMany.


    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }
    
    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



Alle pagina's van de resultaten van de query in de foreach blokken zoals hierboven doorlopen de .NET client automatisch. De query-opties die in de sectie REST API zijn ook beschikbaar in de SDK voor .NET met behulp van de `FeedOptions` en `FeedResponse` klassen in de methode CreateDocumentQuery. Het aantal pagina's kan worden beheerd met behulp van de `MaxItemCount` instelling. 

U kunt ook expliciet paginering bepalen door te maken `IDocumentQueryable` met de `IQueryable` object vervolgens door te lezen de` ResponseContinuationToken` waarden en deze terug als `RequestContinuationToken` in `FeedOptions`. `EnableScanInQuery`scans inschakelen wanneer u de query kan niet worden ondersteund door de geconfigureerde indexing beleid kan worden ingesteld. Voor de gepartitioneerde collecties, kunt u `PartitionKey` de query wilt uitvoeren op een enkele partitie (Hoewel DocumentDB dit automatisch uit de tekst van de query ophalen kan), en `EnableCrossPartitionQuery` voor het uitvoeren van query's die moeten worden uitgevoerd op meerdere partities. 

Verwijzen naar [DocumentDB .NET voorbeelden](https://github.com/Azure/azure-documentdb-net) voor meer voorbeelden met query's. 

### <a name="javascript-server-side-api"></a>Server-side JavaScript-API 
DocumentDB biedt een programmeermodel voor het uitvoeren van JavaScript op basis van toepassingslogica rechtstreeks op de collecties met behulp van opgeslagen procedures en triggers. De JavaScript-logica op een niveau geregistreerd kunt verlenen databasebewerkingen op de bewerkingen in de documenten van de opgegeven collectie. Deze bewerkingen zijn samengevoegd in de ZURE transacties.

In het volgende voorbeeld wordt aangegeven hoe de queryDocuments in de JavaScript-API voor server gebruiken om query's uit binnen opgeslagen procedures en triggers.


    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()
    
        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);
    
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);
    
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a name="aggregate-functions"></a>Statistische functies

Native ondersteuning voor statistische functies in de werken, maar als u in de tussentijd count of SOM-functie, kunt u met behulp van verschillende methoden hetzelfde resultaat bereiken.  

Op pad lezen:

- U kunt statistische functies ophalen van de gegevens en het doen van een aantal lokaal uitvoeren. Het is aanbevolen gebruik een goedkope query projectie zoals `SELECT VALUE 1` in plaats van een volledig document, zoals `SELECT * FROM c`. Dit helpt bij het maximaliseren van het aantal documenten in elke pagina van de resultaten, aanvullende gegevens van en naar de service indien nodig zodat er geen verwerkt.
- U kunt ook een opgeslagen procedure netwerkvertraging op herhaalde retouren minimaliseren. Opgeslagen procedure voor het berekenen van het aantal voor een opgegeven filterquery Zie [Count.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/stored-procedures/Count.js)voor een voorbeeld. De opgeslagen procedure kunt gebruikers combineren rijke bedrijfslogica en aggregaties op efficiënte wijze te doen.

Op pad schrijven:

- Een ander gemeenschappelijk patroon is vooraf de resultaten in het "schrijven" pad samenvoegen. Dit is met name aantrekkelijk als het volume van verzoeken "lezen" hoger dan dat van "write"-aanvragen is. Eenmaal vooraf samengevoegde, de resultaten zijn beschikbaar met één punt leesaanvraag.  De beste manier om een vooraf samengesteld in DocumentDB is een trigger die wordt geactiveerd bij elke "write" instellen en bijwerken van een metagegevensdocument met de meest recente resultaten van de query die wordt wordt gerealiseerd. Bijvoorbeeld, Raadpleeg het [UpdateaMetadata.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/triggers/UpdateMetadata.js) monster, waarbij de minimaal, maxSize en totalSize van het metagegevensdocument voor de collectie bijgewerkt. Het monster kan worden uitgebreid voor het bijwerken van een teller, som, enz.

##<a name="references"></a>Verwijzingen
1.  [Inleiding tot Azure DocumentDB][introduction]
2.  [DocumentDB SQL-specificatie](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3.  [Monsters DocumentDB .NET](https://github.com/Azure/azure-documentdb-net)
4.  [DocumentDB consistentie niveaus][consistency-levels]
5.  ANSI SQL-2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6.  JSON- [http://json.org/](http://json.org/)
7.  JavaScript-specificatie [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8.  LINQ- [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9.  Query-evaluatietechnieken voor grote databases [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Query verwerking in parallelle relationele databasesystemen, IEEE Computer Society Press, 1994
11. Lu, Ooi, Tan, Query verwerking in parallelle relationele databasesystemen, IEEE Computer Society Press, 1994.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: varken Latijn: een niet zo vreemde taal voor gegevensverwerking, SIGMOD 2008.
13.     G. Graefe. Het framework trapsgewijs voor queryoptimalisatie. IEEE gegevens eng Bull., 18, lid 3: 1995.


[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: documentdb-introduction.md
[consistency-levels]: documentdb-consistency-levels.md
 
