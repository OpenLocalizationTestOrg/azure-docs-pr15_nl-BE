<properties 
    pageTitle="Partitionering en schaling in Azure DocumentDB | Microsoft Azure"      
    description="Meer informatie over het maken van partities works in Azure, DocumentDB, hoe partitionering configureren en partitioneren van sleutels en het kiezen van de juiste partitiesleutel voor uw toepassing."         
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/> 

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="arramac"/> 

# <a name="partitioning-and-scaling-in-azure-documentdb"></a>Partitionering en schaling in Azure, DocumentDB
[DocumentDB voor Microsoft Azure](https://azure.microsoft.com/services/documentdb/) is ontworpen om u helpen snel en voorspelbare prestaties en schaal naadloos met uw toepassing als deze gaat groeien. In dit artikel wordt een overzicht gegeven van hoe partitionering werken in DocumentDB en wordt beschreven hoe u DocumentDB collecties wilt schalen effectief uw toepassingen kunt configureren.

Na het lezen van dit artikel, is het mogelijk om de volgende vragen te beantwoorden:   

- Hoe werkt het maken van partities in Azure DocumentDB?
- Hoe configureer partitioneren in DocumentDB
- Wat zijn de partitiesleutels en hoe ik de juiste partitiesleutel kiezen voor mijn toepassing?

Download het project uit [DocumentDB prestaties testen van stuurprogramma monster](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark)om te beginnen met de code. 

## <a name="partitioning-in-documentdb"></a>Partitioneren in DocumentDB

In DocumentDB, kunt u opslaan en schema-less JSON-documenten met de volgorde van milliseconde responstijden met een schaal opvragen. DocumentDB biedt containers voor de opslag van gegevens **collecties**genoemd. Collecties zijn logische bronnen en kunnen een of meer fysieke partities of servers omvatten. Het aantal partities wordt bepaald door de DocumentDB op basis van de omvang en de ingerichte doorvoer van de collectie. Elke partitie in DocumentDB heeft een vaste hoeveelheid SSD-backed opslag gekoppeld en wordt gerepliceerd voor hoge beschikbaarheid. Beheer partitie volledig wordt beheerd door DocumentDB Azure en er geen complexe code schrijven of beheren van uw partities. DocumentDB collecties zijn **vrijwel onbeperkte** opslag en doorvoer. 

Partitioneren is volledig transparant voor de toepassing. DocumentDB ondersteunt snel leest en schrijft, SQL en LINQ-query's op basis van JavaScript transactionele logica, niveaus samenhang en fijnmazig toegangsbeheer via REST API-aanroepen naar een collectie van één resource. De service verwerkt distributie gegevens in partities en query-aanvragen omleiden naar de juiste partitie. 

Hoe werkt dit? Wanneer u een collectie in een DocumentDB maakt, zult u merken dat er is een **belangrijke eigenschap partitie** configuratiewaarde die u opgeeft. Dit is de eigenschap JSON (of het pad) in uw documenten die door DocumentDB kunnen worden gebruikt voor het distribueren van gegevens tussen meerdere servers of partities. DocumentDB wordt een hash-waarde van de partitie en het hash-resultaat gebruiken om te bepalen van de partitie waarin de JSON-document wordt opgeslagen. Alle documenten met dezelfde partitiesleutel worden opgeslagen op dezelfde partitie. 

Neem bijvoorbeeld een toepassing die gegevens over werknemers en hun diensten worden opgeslagen in DocumentDB. We kiezen `"department"` als de belangrijkste eigenschap van partitie te schalen gegevens per afdeling. Elk document in de DocumentDB moet bevatten een verplichte `"id"` uniek zijn voor elk document met dezelfde partitie sleutelwaarde, bijvoorbeeld moet de eigenschap `"Marketing`". Elk document dat is opgeslagen in een collectie moet een unieke combinatie van partitiesleutel en id, bijvoorbeeld `{ "Department": "Marketing", "id": "0001" }`, `{ "Department": "Marketing", "id": "0002" }`, en `{ "Department": "Sales", "id": "0001" }`. Met andere woorden, de samengestelde eigenschap van (sleutel, partitie-id) is de primaire sleutel voor de collectie.

### <a name="partition-keys"></a>Partitie-toetsen
De keuze van de partitiesleutel is een belangrijke beslissing die hebt u tijdens het ontwerpen maken. U moet de naam van een JSON-eigenschap die een breed scala van waarden en evenredig patronen toegang zal kiezen. De partitiesleutel is opgegeven als een JSON-pad, bijvoorbeeld `/department` de afdeling eigenschap vertegenwoordigt. 

In de volgende tabel ziet u voorbeelden van de belangrijkste definities partitie en de JSON-waarden die overeenkomen met elk.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Het pad van partitie</strong></p></td>
            <td valign="top"><p><strong>Beschrijving</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Komt overeen met de JSON-waarde van doc.department doc waar het document is.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Eigenschappen/naam</p></td>
            <td valign="top"><p>Komt overeen met de JSON-waarde van doc.properties.name waarbij doc het document (eigenschap genest is).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Komt overeen met de JSON-waarde van doc.id (partitie-id en de sleutel zijn dezelfde eigenschap).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ 'Afdelingsnaam'</p></td>
            <td valign="top"><p>Komt overeen met de waarde JSON van doc ['Afdelingsnaam'] doc waar het document is.</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] De syntaxis voor het pad van partitie is vergelijkbaar met het opgegeven pad voor het indexeren van beleid paden met het belangrijkste verschil is dat het pad overeenkomt met de eigenschap de waarde, dat wil zeggen er is geen wild creditcard aan het einde. Zo geeft u afdeling /? de waarden onder afdeling indexeert, maar u /department als de definitie van de partitie opgeven. Het pad van de partitie wordt impliciet geïndexeerd en indexeert indexing beleid overschrijvingen gebruik kan niet worden uitgesloten.

We gaan kijken hoe de keuze van de partitiesleutel van invloed op de prestaties van uw toepassing.

### <a name="partitioning-and-provisioned-throughput"></a>Partitionering en ingerichte doorvoer
DocumentDB is ontworpen voor voorspelbare prestaties. Als u een verzameling maakt, kunt u reserveren doorvoersnelheid uitgedrukt in ** [eenheden van de aanvraag](documentdb-request-units.md) (RU) per seconde**. Elke aanvraag wordt toegewezen aanvraag eenheid kosten die evenredig is aan het bedrag van de systeembronnen, zoals CPU en i/o verbruikt door de bewerking. Het lezen van een document van 1 kB met consistentie sessie verbruikt 1 verzoek eenheid. Lees is 1 RU ongeacht het aantal opgeslagen items of het aantal gelijktijdige aanvragen dat tegelijkertijd wordt uitgevoerd. Grotere documenten vereisen hogere aanvraag eenheden afhankelijk van de grootte. Als u de grootte van de entiteiten en het aantal gelezen, moet u ondersteuning voor uw toepassing, kunt u het exacte bedrag van doorvoer die vereist is voor uw toepassing nodig heeft vindt inrichten. 

Wanneer DocumentDB worden documenten opgeslagen, verdeeld deze gelijkmatig uit op basis van de waarde van de partitie partities. De doorvoer wordt ook gelijkmatig verdeeld over de beschikbare partities dat wil zeggen de doorvoer per partitie = (totale doorvoer per collectie) / (aantal partities). 

>[AZURE.NOTE] Met het oog op de volledige doorvoer van de collectie, moet u een partitiesleutel op kunt u aanvragen bij een aantal waarden van de afzonderlijke partitie gelijkmatig te verdelen.

## <a name="single-partition-and-partitioned-collections"></a>Één partitie en gepartitioneerde collecties
DocumentDB ondersteunt het maken van één partitie en het gepartitioneerde collecties. 

- **Partitioned collecties** kunnen meerdere partities omvatten en ondersteuning voor zeer grote hoeveelheden opslag en doorvoer. U moet een partitiesleutel opgeven voor de collectie.
- **Collecties met één partitie** hebt lagere prijs opties en mogelijkheden voor het opvragen en transacties uitvoeren via alle gegevens te verzamelen. Zij hebben de grenzen voor schaalbaarheid en opslag van één partitie. U beschikt niet over een partitiesleutel voor deze collecties op te geven. 

![Gepartitioneerde collecties in DocumentDB][2] 

Collecties met één partitie zijn scenario's die niet te worden grote hoeveelheden opslag of doorvoer hoeven thuishoort. Collecties met één partitie hebben de schaalbaarheid en de grenzen van de opslag van een enkele partitie, dat wil zeggen 10 GB van opslag en eenheden van 10.000 aanvragen per seconde. 

Gepartitioneerde collecties kunnen zeer grote hoeveelheden opslag en doorvoer te ondersteunen. De standaard biedt zijn geconfigureerd voor het opslaan van maximaal 250 GB opslagcapaciteit en schalen tot 250.000 aanvraag eenheden per seconde. Als u hogere opslag of doorvoer per collectie, neem contact op met [Ondersteuning van Azure](documentdb-increase-limits.md) als u wilt dat deze betere voor uw account.

De volgende tabel worden de verschillen in het werken met één partitie en gepartitioneerde collecties:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Één partitie, collectie</strong></p></td>
            <td valign="top"><p><strong>Gepartitioneerde collectie</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Partitiesleutel</p></td>
            <td valign="top"><p>Geen</p></td>
            <td valign="top"><p>Vereist</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Primaire sleutel voor een Document</p></td>
            <td valign="top"><p>"id"</p></td>
            <td valign="top"><p>samengestelde sleutel &lt;partitiesleutel&gt; en "id"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimale opslag</p></td>
            <td valign="top"><p>0 GB</p></td>
            <td valign="top"><p>0 GB</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximale opslagcapaciteit</p></td>
            <td valign="top"><p>10 GB</p></td>
            <td valign="top"><p>Onbeperkt (250 GB standaard)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimale doorvoer</p></td>
            <td valign="top"><p>400 eenheden voor aanvragen per seconde</p></td>
            <td valign="top"><p>eenheden van 10.000 aanvragen per seconde</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximale doorvoer</p></td>
            <td valign="top"><p>eenheden van 10.000 aanvragen per seconde</p></td>
            <td valign="top"><p>Onbeperkt (aanvraag 250.000 eenheden per seconde standaard)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>API-versies</p></td>
            <td valign="top"><p>Alle</p></td>
            <td valign="top"><p>API 2015-12-16 en nieuwer</p></td>
        </tr>
    </tbody>
</table>

## <a name="working-with-the-sdks"></a>Werken met de SDK 's

Azure DocumentDB ondersteuning toegevoegd voor automatische partitioneren met [REST API versie 2015-12-16](https://msdn.microsoft.com/library/azure/dn781481.aspx). Om de gepartitioneerde verzamelingen maken, moet u de SDK versie 1.6.0 downloaden of later op een van de ondersteunde platforms SDK (.NET, Node.js, Java, Python). 

### <a name="creating-partitioned-collections"></a>Gepartitioneerde verzamelingen maken

In het volgende voorbeeld ziet u een fragment .NET voor het maken van een collectie voor de opslag apparaat telemetriegegevens van 20.000 eenheden voor aanvraag per seconde van doorvoer. De SDK stelt de waarde van de OfferThroughput (die op zijn beurt stelt de `x-ms-offer-throughput` verzoek-header in de REST API). Hier stelt de `/deviceId` als de partitiesleutel. De keuze van de partitiesleutel wordt opgeslagen samen met de rest van de collectie metagegevens, zoals naam en beleid voor indexering.

Voor dit monster verzameld we `deviceId` omdat we dat weten (a) en omdat er een groot aantal apparaten, schrijfbewerkingen kunnen meerdere partities gelijkmatig worden verdeeld en ons toestemming te schalen van de database om de consumptie van enorme hoeveelheden gegevens en (b) veel van de aanvragen als het ophalen van het meest recente lezen voor een apparaat zijn binnen het bereik van een enkele deviceId en kunnen worden opgehaald uit een enkele partitie.

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] Om de gepartitioneerde verzamelingen maken, moet u een doorvoercapaciteit van 10.000 > aanvraag eenheden per seconde. Aangezien de doorvoer in veelvouden van 100, is dit 10,100 of hoger.

Deze methode maakt een REST API aanroepen van DocumentDB en de service zal een aantal partities op basis van de aangevraagde doorvoer inrichten. De prestaties van uw behoeften evolueren, kunt u de doorvoer van een collectie wijzigen. [Prestaties](documentdb-performance-levels.md) Zie voor meer informatie.

### <a name="reading-and-writing-documents"></a>Lezen en schrijven van documenten

Nu gaan we gegevens invoegen in DocumentDB. Hier is een voorbeeldklasse met een apparaat lezen en een oproep aan CreateDocumentAsync om het invoegen van een nieuw apparaat lezen in een collectie.

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


We lezen het document door de partitiesleutel-id, bijwerken en een laatste stap Verwijder deze partitiesleutel en id. Opmerking dat het lezen een PartitionKey-waarde bevatten (die overeenkomt met de `x-ms-documentdb-partitionkey` verzoek-header in de REST API).

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### <a name="querying-partitioned-collections"></a>Bij het controleren van gepartitioneerde collecties

Wanneer u de gegevens in de gepartitioneerde verzamelingen gegevens DocumentDB de query automatisch omgeleid naar de partities die overeenkomt met de partitie-sleutelwaarden in de filter hebt opgegeven (als die er zijn). Deze query wordt bijvoorbeeld doorgestuurd naar partitie met de partitiesleutel "XMS-0001".

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

De volgende query heeft geen filter op de partitie-toets (DeviceId) en is fanned uit tot alle partities waarbij wordt uitgevoerd ten opzichte van de partitie-index. Opmerking u hebt voor het opgeven van de EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in de REST API) hebben de SDK voor het uitvoeren van een query meerdere partities.

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### <a name="parallel-query-execution"></a>Parallelle Query Execution

De DocumentDB SDK's 1.9.0 en boven de parallelle query execution ondersteuningsopties, waarmee u lage latentie query's kan uitvoeren tegen de gepartitioneerde collecties, zelfs wanneer ze nodig hebben om een groot aantal partities aanraken. De volgende query is bijvoorbeeld geconfigureerd voor meerdere partities parallel uitgevoerd.

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

U kunt de parallelle query execution beheren door het afstemmen de volgende parameters:

- Door `MaxDegreeOfParallelism`, kunt u de mate van parallellisme, dat wil zeggen, het maximum aantal gelijktijdige netwerkverbindingen met partities van de collectie bepalen. Als u deze instelt op -1, de mate van parallellisme wordt beheerd door de SDK. Als de `MaxDegreeOfParallelism` is niet opgegeven of is ingesteld op 0, de standaardwaarde is, wordt er één netwerkverbinding partities van de collectie.
- Door `MaxBufferedItemCount`, kunt u zaken uit query latentie en client side geheugengebruik. Als u deze parameter weglaat of deze instelt op -1, het aantal artikelen tijdens de parallelle query execution worden gebufferd wordt beheerd door de SDK.

Uitgaande van dezelfde staat van de collectie, retourneert een parallelle query resultaten in dezelfde volgorde als in seriële uitvoering. Bij het uitvoeren van een cross-partitie query sorteren (sorteren en/of boven), wordt de DocumentDB SDK problemen met de query gelijktijdig meerdere partities en gedeeltelijk gesorteerde resultaten in de client globaal bestelde resultaten worden samengevoegd.

### <a name="executing-stored-procedures"></a>Opgeslagen procedures uitvoeren

Kunt u ook uitvoeren atomische transacties tegen documenten met hetzelfde apparaat-ID, bijvoorbeeld als u statistische functies of de meest recente status van een apparaat in een enkel document onderhoudt. 

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

We bekijken hoe u naar de gepartitioneerde collecties uit één partitie collecties verplaatsen kunt in de volgende sectie.

<a name="migrating-from-single-partition"></a>
### <a name="migrating-from-single-partition-to-partitioned-collections"></a>Overstappen van één partitie op de gepartitioneerde collecties
Wanneer een toepassing op basis van een collectie één partitie moet hogere doorvoer (> 10.000 RU/s) of grotere gegevensopslag (> 10GB), kunt u de [DocumentDB Data Migration Tool](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) voor het migreren van de gegevens uit de collectie één partitie op een gepartitioneerde collectie. 

Een collectie van één partitie migreren naar een gepartitioneerde collectie

1. Gegevens exporteren uit de collectie één partitie naar JSON. Zie [exporteren naar JSON-bestand](documentdb-import-data.md#export-to-json-file) voor meer informatie.
2. Importeer de gegevens in een gepartitioneerde collectie die is gemaakt met de definitie van een partitie en meer dan 10.000 aanvragen eenheden per tweede doorvoer, zoals in het volgende voorbeeld wordt getoond. Zie [importeren naar DocumentDB](documentdb-import-data.md#DocumentDBSeqTarget) voor meer informatie.

![Gegevens migreren naar een collectie Partitioned in DocumentDB][3]  

>[AZURE.TIP] Voor snellere tijden voor importeren, kunt u verhoogt het nummer van parallelle verzoeken tot 100 of hoger om te profiteren van de hogere doorvoer beschikbaar voor de gepartitioneerde collecties. 

Nu dat we de grondbeginselen hebben voltooid, we kijken naar enkele belangrijke overwegingen bij het werken met de partitiesleutels in DocumentDB.

## <a name="designing-for-partitioning"></a>Voor het partitioneren van ontwerpen
De keuze van de partitiesleutel is een belangrijke beslissing die hebt u tijdens het ontwerpen maken. Deze sectie worden enkele van de compromissen die erbij betrokken zijn bij het selecteren van een partitiesleutel voor uw verzameling.

### <a name="partition-key-as-the-transaction-boundary"></a>De partitiesleutel als de rand van de transactie
Uw keuze van partitiesleutel moet de noodzaak om het gebruik van de transacties tegen de eis om de entiteiten verdelen over meerdere partitiesleutels om een schaalbare oplossing in evenwicht. U kan dezelfde partitiesleutel instellen voor al uw documenten op een extreme, maar dit de schaalbaarheid van uw oplossing kan beperken. Het andere uiterste, kunt u een met unieke partitiesleutel voor elk document dat zou zijn uiterst schaalbaar, maar zou verhinderen dat u met behulp van cross-document transacties via opgeslagen procedures en triggers toewijzen. Een ideale partitie-toets is een die kunt u met efficiënte query's en die voldoende kardinaliteit zodat uw oplossing is schaalbaar is. 

### <a name="avoiding-storage-and-performance-bottlenecks"></a>Opslag- en knelpunten te voorkomen 
Het is ook belangrijk dat u een eigenschap die kan worden verdeeld over een aantal verschillende waarden kiezen. Aanvragen voor dezelfde partitiesleutel mag niet meer dan de doorvoer van een enkele partitie en wordt de snelheid. Het is dus belangrijk dat u een partitie gebruikt die niet leidt een **"hotspots"** binnen de toepassing tot te kiezen. De totale opslagruimte voor documenten met dezelfde partitiesleutel kan ook niet meer dan 10 GB in opslag. 

### <a name="examples-of-good-partition-keys"></a>Voorbeelden van goede partitiesleutels
Hier volgen enkele voorbeelden van hoe kiest u de partitiesleutel voor uw toepassing:

* Als u een gebruiker profiel backend implementeert bent, is de gebruikers-ID een goede keuze voor de partitiesleutel.
* Als u bent IoT gegevens, zoals de status van het apparaat op te slaan, is een apparaat-ID een goede keuze voor de partitiesleutel.
* Als u DocumentDB gebruikt voor het vastleggen van tijd-serie, is de hostnaam of het proces-ID een goede keuze voor de partitiesleutel.
* Als u beschikt over een architectuur met meerdere huurder, is de ID van de huurder een goede keuze voor de partitiesleutel.

Houd er rekening mee dat in sommige gevallen gebruik (zoals de IoT en gebruikers profielen hierboven), de partitiesleutel mogelijk hetzelfde als uw id (documentsleutel). In andere gevallen als de reeksgegevens tijd wellicht de partitiesleutel van een die anders is dan de id.

### <a name="partitioning-and-loggingtime-series-data"></a>Partitionering en logboekregistratie-tijdreeks
Een van de meest voorkomende gevallen gebruik van DocumentDB is voor logboekregistratie en telemetrie. Het is belangrijk dat een goede partitiesleutel kiezen omdat u mogelijk grote hoeveelheden gegevens lezen/schrijven. De keuze zal afhankelijk zijn van het lezen en schrijven van tarieven en soorten query's die u verwacht te worden uitgevoerd. Hier volgen enkele tips over het kiezen van een goede partitiesleutel.

- Als sprake is van de use-case een klein percentage van acculumating schrijft gedurende een lange periode van tijd en moet de query door het bereiken van tijdstempels en andere filters, kan het gebruik van een combinatie van de tijdstempel bv. datum als de partitiesleutel van een een goede aanpak is. Hierdoor kunt u query over alle gegevens voor een datum uit een enkele partitie. 
- Als uw werkbelasting schrijven dik, die over het algemeen vaker, moet u een partitie gebruikt die niet gebaseerd op het tijdstempel dat DocumentDB schrijft over een aantal partities verdelen kunt. Een hostnaam, proces-ID, activiteit-ID of een andere eigenschap met een hoge kardinaliteit is hier een goede keuze. 
- Een derde benadering is een hybride een waar er meerdere collecties, één voor elke dag/maand en de partitiesleutel is een gedetailleerde eigenschap als hostnaam. Dit heeft als voordeel dat u kunt verschillende prestatieniveaus op basis van het tijdvenster instellen, bijvoorbeeld de collectie voor de huidige maand is ingericht met een hogere doorvoersnelheid omdat het Lees- en schrijfbewerkingen, dient dat voorafgaande maanden met lagere doorvoersnelheid omdat ze alleen leesbewerkingen dienen.

### <a name="partitioning-and-multi-tenancy"></a>Partitionering en meerdere pacht
Als u een DocumentDB met meerdere huurder-toepassing implementeert, zijn er twee belangrijke patronen voor de uitvoering van de pacht met DocumentDB – sleutel één partitie per huurder en één collectie per huurder. Hier vindt u de voor- en nadelen voor elk:

* Één partitiesleutel per huurder: In dit model, de huurders zijn collocated binnen één collectie. Maar de query's en inlegvellen voor documenten binnen een enkele huurder kunnen worden uitgevoerd met één partitie. Transactionele logica te implementeren in alle documenten in een huurder. Omdat meerdere huurders een verzameling deelt, kunt u de opslag en doorvoer kosten besparen door bundeling van middelen voor huurders in één collectie in plaats van extra ruimte voor elke huurder ingericht. Het nadeel is dat er geen isolatie prestaties per huurder. Prestaties/doorvoer verhogingen van toepassing op de hele collectie vs gerichte verhogingen voor huurders.
* Een collectie per huurder: elke huurder heeft een eigen collectie. In dit model, kunt u de prestaties per huurder reserveren. Dit model is met de DocumentDB van nieuwe op basis van verbruik prijsmodel, rendabeler voor huurder met meerdere toepassingen met een klein aantal huurders.

U kunt ook een combinatie/gelaagde benadering die collocates kleine pachters en huurders groter migreert naar hun eigen collectie gebruiken.

## <a name="next-steps"></a>Volgende stappen
In dit artikel beschreven we hoe partitionering works in Azure, DocumentDB, hoe u collecties van gepartitioneerde kunt maken en hoe u een goede partitiesleutel kunt kiezen voor uw toepassing. 

-   Schaal en prestaties testen met DocumentDB uitvoeren. Zie [prestaties en schaal testen met Azure DocumentDB](documentdb-performance-testing.md) voor een voorbeeld.
-   Aan de slag met de [SDK's](documentdb-sdk-dotnet.md) of de [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) -codering
-   Meer informatie over [ingerichte doorvoer in DocumentDB](documentdb-performance-levels.md)
-   Als u aanpassen hoe partitioneren van uw toepassing uitvoert wilt, kunt u in uw eigen partitionering implementatie van client-side aansluiten. Zie [Client-side partititie-ondersteuning](documentdb-sharding.md).

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png  

 
