<properties 
    pageTitle="Werken met georuimtelijke gegevens in Azure DocumentDB | Microsoft Azure" 
    description="Weten hoe te maken, te indexeren en ruimtelijke objecten met Azure DocumentDB query." 
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/25/2016" 
    ms.author="arramac"/>
    
# <a name="working-with-geospatial-data-in-azure-documentdb"></a>Werken met georuimtelijke gegevens in Azure, DocumentDB

Dit artikel vormt een inleiding tot de functionaliteit voor georuimtelijke in [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Na het lezen van dit, is het mogelijk om de volgende vragen te beantwoorden:

- Hoe kan ik de ruimtelijke data in Azure DocumentDB opslaan?
- Hoe kan ik een georuimtelijke gegevens in Azure DocumentDB in SQL en LINQ query?
- Hoe ik in- of uitschakelen ruimtelijke indexeren in DocumentDB?

Zie dit [Github project](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) voor voorbeelden van programmacode.

## <a name="introduction-to-spatial-data"></a>Inleiding tot de ruimtelijke gegevens

Ruimtelijke data beschrijft de positie en vorm van objecten in de ruimte. In de meeste toepassingen, deze komen overeen met objecten op de aarde, dat wil zeggen georuimtelijke gegevens. Ruimtelijke gegevens kunnen worden gebruikt voor het weergeven van de locatie van een persoon, een plaats van belang of de grens van een stad of een meer. Nabijheid van query's voor bijvoorbeeld 'Zoek alle cafés in de buurt van mijn huidige locatie' vaak betrekking hebben op algemene use-cases. 

### <a name="geojson"></a>GeoJSON
DocumentDB ondersteunt het indexeren en doorzoeken van georuimtelijke gegevens die worden weergegeven met behulp van de [GeoJSON-specificatie](http://geojson.org/geojson-spec.html). Gegevensstructuren GeoJSON zijn altijd geldig JSON-objecten, zodat ze kunnen worden opgeslagen en opgevraagd zonder speciale hulpprogramma's of bibliotheken met behulp van DocumentDB. De DocumentDB SDK's bieden helper klassen en methoden waarmee u gemakkelijk werken met ruimtelijke gegevens. 

### <a name="points-linestrings-and-polygons"></a>Punten, linestrings en veelhoeken
Een **punt** geeft een één positie in de ruimte. In georuimtelijke gegevens staat een punt voor de exacte locatie die een adres van een supermarkt, een kiosk, een auto of een stad kan zijn.  Een punt wordt weergegeven in een paar GeoJSON (en DocumentDB) met behulp van de coördinaten of de lengte en breedte. Hier volgt een voorbeeld van JSON voor een punt.

**Punten in DocumentDB**

    {
       "type":"Point",
       "coordinates":[ 31.9, -4.8 ]
    }

>[AZURE.NOTE] De specificatie van de GeoJSON geeft de geografische lengte- en latitude eerste tweede. Net als in andere toepassingen toewijzen, lengte en breedte worden hoeken en uitgedrukt in graden weergegeven. Lengtegraad waarden worden gemeten vanaf de eerste meridiaan behoren en tussen-180 en 180.0 graden en latitude zijn waarden worden gemeten vanaf de evenaar en tussen-90.0 en 90.0 graden. 
>
> DocumentDB interpreteert coördinaten zoals deze worden weergegeven per het referentiesysteem WGS 84. Zie hieronder voor meer informatie over referentiesystemen coördineren.

Dit kan worden ingesloten in een document met DocumentDB zoals in dit voorbeeld van een profiel met locatiegegevens:

**Gebruik profiel opgeslagen in DocumentDB**

    {
       "id":"documentdb-profile",
       "screen_name":"@DocumentDB",
       "city":"Redmond",
       "topics":[ "NoSQL", "Javascript" ],
       "location":{
          "type":"Point",
          "coordinates":[ 31.9, -4.8 ]
       }
    }

Naast de punten ondersteunt GeoJSON ook LineStrings en veelhoeken. **LineStrings** vertegenwoordigen een reeks van twee of meer punten in de ruimte en de lijnsegmenten die ze. Georuimtelijke gegevens, worden de linestrings vaak gebruikt voor hoofdwegen of rivieren. Een **Veelhoek** is een grens van verbonden punten die een gesloten LineString vormt. Veelhoeken worden vaak gebruikt voor natuurlijke formaties zoals meren of politieke jurisdicties zoals steden en Staten. Hier volgt een voorbeeld van een veelhoek in de DocumentDB. 

**Veelhoeken in DocumentDB**

    {
       "type":"Polygon",
       "coordinates":[
           [ 31.8, -5 ],
           [ 31.8, -4.7 ],
           [ 32, -4.7 ],
           [ 32, -5 ],
           [ 31.8, -5 ]
       ]
    }

>[AZURE.NOTE] De specificatie van de GeoJSON moet voor geldige veelhoeken, het laatste coördinaat paar die hetzelfde is als de eerste, een gesloten shape maken.
>
>Punten binnen een veelhoek moeten worden opgegeven in volgorde linksom. De inverse van de regio in het Hiermee geeft u een veelhoek die is opgegeven in de order met de klok mee.

Point, LineString en veelhoek bepaalt GeoJSON ook de weergave voor het groeperen van meerdere locaties voor georuimtelijke als willekeurige eigenschappen koppelen aan geolocation als een **functie**. Aangezien deze objecten geldig JSON zijn, kunnen alle worden opgeslagen en verwerkt in DocumentDB. Echter DocumentDB ondersteunt alleen de automatische indexering van punten.

### <a name="coordinate-reference-systems"></a>Coördineren van de referentiesystemen

Aangezien de vorm van de aarde onregelmatig is, wordt veel coördinaat referentiesystemen (CRS), elk met hun eigen frames van verwijzing en maateenheden coördinaten van georuimtelijke gegevens weergegeven. De "nationale raster van-Brittannië' is bijvoorbeeld een referentiesysteem zeer nauwkeurig is voor het Verenigd Koninkrijk, maar niet buiten. 

De meest populaire CRS in gebruik is vandaag de dag de wereld geodetisch systeem [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). GPS-apparaten en veel toewijzing services zoals Google Maps en de Bing Maps-API's gebruiken WGS 84. DocumentDB ondersteunt het indexeren en doorzoeken van georuimtelijke gegevens met behulp van alleen de WGS 84-Boekingssysteem. 

## <a name="creating-documents-with-spatial-data"></a>Documenten maken met ruimtelijke gegevens
Bij het maken van documenten die GeoJSON waarden bevatten, worden deze automatisch geïndexeerd met een ruimtelijke index volgens het beleid voor indexering van de collectie. Als u met een DocumentDB SDK in een dynamisch getypeerde talen zoals Python of Node.js werkt, moet u geldige GeoJSON.

**Document met georuimtelijke gegevens in Node.js maken**

    var userProfileDocument = {
       "name":"documentdb",
       "location":{
          "type":"Point",
          "coordinates":[ -122.12, 47.66 ]
       }
    };

    client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
        // additional code within the callback
    });

Als u met de .NET (of Java) SDK's werkt, kunt u het nieuwe punt en Veelhoek klassen in de naamruimte Microsoft.Azure.Documents.Spatial locatie-informatie in uw toepassingsobjecten insluiten. Deze klassen vereenvoudigen serialisatie en deserialisatie van ruimtelijke gegevens in GeoJSON.

**Document met georuimtelijke gegevens in .NET maken**

    using Microsoft.Azure.Documents.Spatial;
    
    public class UserProfile
    {
        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("location")]
        public Point Location { get; set; }
        
        // More properties
    }
    
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
        new UserProfile 
        { 
            Name = "documentdb", 
            Location = new Point (-122.12, 47.66) 
        });

Als u niet beschikt over de breedte- en lengtewaarden informatie, maar de fysieke adressen of de naam van de locatie zoals een plaats of land hebt, kunt u de werkelijke coördinaten opzoeken met behulp van een geocoding service zoals Bing Maps REST Services. Meer informatie over Bing Maps geocoding [hier](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Ruimtelijke typen query 's

Nu dat we kijken hoe u kunt georuimtelijke gegevens invoegen hebt gemaakt, laten we kijken hoe u deze gegevens met behulp van DocumentDB met behulp van SQL en LINQ query.

### <a name="spatial-sql-built-in-functions"></a>Ruimtelijke ingebouwde SQL-functies
DocumentDB ondersteunt de volgende ingebouwde functies voor Open georuimtelijke Consortium (OGC) voor georuimtelijke opvragen. Raadpleeg voor meer informatie over de volledige set van ingebouwde functies in de SQL-taal, de [Query DocumentDB](documentdb-sql-query.md).

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

Ruimtelijke functies kunnen worden gebruikt voor het uitvoeren van de nabijheid van query's op ruimtelijke gegevens. Hier is bijvoorbeeld een query die alle familie documenten die binnen 30 km van de opgegeven locatie met behulp van de ingebouwde functie voor ST_DISTANCE. 

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

Veelhoek argumenten in ST_WITHIN slechts één keer kunnen bevatten, dat wil zeggen de veelhoeken mag geen gaten erin. 

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

DocumentDB biedt ook ondersteuning voor inverse query's uitvoeren, dat wil zeggen kunt u veelhoeken of regels in DocumentDB index en vervolgens zoeken naar gebieden met een opgegeven punt. Dit patroon wordt vaak gebruikt in de logistiek om aan te duiden, bijvoorbeeld wanneer een vrachtwagen invoert of een aangewezen gebied verlaat. 

**Query**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Resultaten**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

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
    
### <a name="linq-querying-in-the-net-sdk"></a>LINQ-query's in de SDK voor .NET

De DocumentDB .NET SDK ook providers stub-methoden `Distance()` en `Within()` voor gebruik binnen het LINQ-expressies. De DocumentDB LINQ provider vertaalt deze methodeaanroepen naar de equivalente SQL-ingebouwde functieaanroepen (ST_DISTANCE en ST_WITHIN respectievelijk). 

Hier volgt een voorbeeld van een LINQ query alle documenten in de collectie van de DocumentDB waarvan de waarde 'locatie vindt' is binnen een straal van 30km van het opgegeven punt met LINQ.

**LINQ-query voor afstand**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Hier is ook een query voor het zoeken naar alle documenten waarvan 'locatie' binnen de opgegeven vak/veelhoek is. 

**LINQ query voor binnen**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Nu dat we kijken hoe u documenten met LINQ en SQL query hebt gemaakt, laten we bij het configureren van DocumentDB voor het indexeren van ruimtelijke.

## <a name="indexing"></a>Indexeren

Wij, zoals in het [Schema Agnostic indexering met Azure DocumentDB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papier beschreven we ontworpen database-engine van de DocumentDB zijn echt schema agnostic en eerste klas ondersteuning bieden voor JSON. De database-engine geoptimaliseerde schrijven van DocumentDB begrijpt native ruimtelijke data (punten, veelhoeken en regels) die wordt weergegeven in de standaard GeoJSON.

Kortom, de geometrie wordt geprojecteerd van geodetisch coördinaten op een 2D-vlak en vervolgens geleidelijk wordt opgesplitst in cellen met een **quadtree**. Deze cellen worden toegewezen aan 1D op basis van de locatie van de cel binnen een **Hilbert ruimte vullen curve**, die de plaats van de punten blijft behouden. Ook wanneer andere locatiegegevens wordt geïndexeerd, wordt er een zogeheten **mozaïekpatroon**, dat wil zeggen alle cellen die dwars op een locatie worden geïdentificeerd en opgeslagen als sleutels in de index DocumentDB. Bij query zijn argumenten zoals punten en veelhoeken ook representatie mozaïekpatroon om uit te pakken van de betreffende cel-ID bereiken, wordt gebruikt voor het ophalen van gegevens uit de index.

Als u een indexering beleid met ruimtelijke-index / * (alle paden) en vervolgens alle punten gevonden binnen de collectie zijn geïndexeerd voor efficiënte ruimtelijke-query's (ST_WITHIN en ST_DISTANCE). Ruimtelijke indexen niet een waarde voor precisie en gebruik altijd een standaardwaarde voor precisie.

>[AZURE.NOTE] DocumentDB ondersteunt automatische indexering van punten, veelhoeken en LineStrings

Het volgende JSON-fragment toont een indexing beleid met ruimtelijke indexeren ingeschakeld, dat wil zeggen index nergens GeoJSON gevonden in documenten voor ruimtelijke opvragen. Als u de indexing-beleid met behulp van de Portal Azure wijzigt, kunt u de volgende JSON voor indexing beleid inschakelen ruimtelijke indexering van de collectie.

**Collectie Indexing JSON met spatiaal ingeschakeld voor punten en veelhoeken beleid**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Hier is een stukje code in .NET waarin wordt uitgelegd hoe u een collectie maken met ruimtelijke indexeren is ingeschakeld voor alle paden die punten bevatten. 

**Een collectie maken met ruimtelijke indexeren**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

En hier is hoe u een bestaande collectie om te profiteren van de ruimtelijke indexering over alle punten die zijn opgeslagen in documenten kunt wijzigen.

**Wijzigen van een bestaande collectie met ruimtelijke indexeren**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [AZURE.NOTE] Als de locatie van de waarde GeoJSON in het document onjuist of ongeldig is, wordt vervolgens het niet geïndexeerd voor ruimtelijke opvragen. U kunt valideren locatie waarden met behulp van ST_ISVALID en ST_ISVALIDDETAILED.
>
> Als de definitie van de collectie een partitiesleutel bevat, wordt indexing transformatie voortgang niet vermeld. 

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt vernomen over hoe aan de slag met ondersteuning voor georuimtelijke in DocumentDB, kunt u het volgende doen:

- Start het coderen met de [georuimtelijke .NET codevoorbeelden op Github](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
- Handen op te halen met georuimtelijke opvragen bij de [DocumentDB Query Speelplaats](http://www.documentdb.com/sql/demo#geospatial)
- Meer informatie over [De DocumentDB Query](documentdb-sql-query.md)
- Meer informatie over het [Beleid voor het indexeren van DocumentDB](documentdb-indexing-policies.md)
