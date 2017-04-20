<properties 
    pageTitle="DocumentDB .NET API en SDK | Microsoft Azure" 
    description="Meer weten over de .NET API en SDK, inclusief release datums, pensioen, datums en wijzigingen die zijn aangebracht tussen elke versie van de DocumentDB .NET SDK." 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-API's en SDK 's 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>DocumentDB .NET API en SDK

<table>
<tr><td>**SDK downloaden**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**API-documentatie**</td><td>[.NET API-documentatie](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**Voorbeelden**</td><td>[Voorbeelden van .NET-code](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Aan de slag**</td><td>[Aan de slag met de DocumentDB .NET SDK](documentdb-get-started.md)</td></tr>
<tr><td>**Zelfstudie op het web app**</td><td>[Ontwikkeling van webtoepassingen met DocumentDB](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Huidige ondersteunde framework**</td><td>[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Release-opmerkingen

> [AZURE.IMPORTANT] Vanaf versie 1.9.2 wordt System.NotSupportedException bij het opvragen van gepartitioneerde collecties. Als u wilt voorkomen dat deze fout, zorgen ervoor dat het hostproces voor 64-bits. Uitvoerbare projecten, kan dit worden gedaan door het uitschakelen van de optie 'Prefer 32-bits' in het venster project eigenschappen op het tabblad opbouwen.

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - Directe verbinding toegevoegde ondersteuning voor gepartitioneerde collecties.
  - Verbeterde prestaties voor het niveau van de consistentie van Staleness wordt begrensd.
  - Veelhoek en toegevoegd LineString DataTypes collectie indexing beleid voor ruimtelijke geo-hekwerk-query's te specificeren.
  - LINQ ondersteuning toegevoegd voor StringEnumConverter, IsoDateTimeConverter en UnixDateTimeConverter tijdens het omzetten van predikaten.
  - Verschillende correcties voor SDK.

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Een probleem dat de volgende NotFoundException veroorzaakt vastgesteld: de sessie gelezen is niet beschikbaar voor de invoer sessietoken. Dit is een uitzondering opgetreden in sommige gevallen bij het opvragen van regio lezen van een account geo verdeeld.
  - De eigenschap ResponseStream in de klasse ResourceResponse kunt rechtstreeks toegang naar de onderliggende stroom via een reactie wordt blootgesteld.

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - De klassen ResourceResponse, FeedResponse, StoredProcedureResponse en MediaResponse om de overeenkomstige openbare interface implementeren zodat ze kunnen worden mocked voor test driven implementatie (TDD) gewijzigd.
  - Een probleem waardoor een ongeldige partitie sleutel kop met behulp van een aangepaste JsonSerializerSettings-object voor het serialiseren gegevens vast.

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Langdurige query's mislukken met de fout veroorzaakt een probleem opgelost: Autorisatietoken is niet geldig bij de huidige tijd.
  - Een probleem dat wordt verwijderd van de oorspronkelijke SqlParameterCollection van cross-partitie boven/order-door query's vast.

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - Toegevoegde ondersteuning voor parallelle query's voor de gepartitioneerde collecties.
  - Ondersteuning toegevoegd voor cross-partitie sorteren en TOP-query's voor de gepartitioneerde collecties.
  - Vaste ontbrekende verwijzingen naar DocumentDB.Spatial.Sql.dll en Microsoft.Azure.Documents.ServiceInterop.dll die nodig zijn bij het verwijzen naar een DocumentDB project met een verwijzing naar het pakket DocumentDB Nuget.
  - De mogelijkheid om de parameters van de verschillende typen gebruiken wanneer u door de gebruiker gedefinieerde functies in LINQ vast. 
  - Vast een bug voor globaal gerepliceerde rekeningen waar Upsert aanroepen zijn worden doorgestuurd naar locaties in plaats van schrijven locaties lezen.
  - Extra methoden aan de IDocumentClient-interface die ontbreken: 
      - UpsertAttachmentAsync-methode wordt mediaStream opties als parameters
      - CreateAttachmentAsync methode die opties als een parameter wordt
      - Methode voor CreateOfferQuery die querySpec als een parameter nodig is.
  - Public, klassen die beschikbaar zijn in de interface IDocumentClient.

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - De ondersteuning voor meerdere regio database accounts toegevoegd.
  - Ondersteuning toegevoegd voor nieuwe aanvragen beperkt.  Gebruiker kan het aantal pogingen en de maximale wachttijd aanpassen door de eigenschap ConnectionPolicy.RetryOptions.
  - Een nieuwe IDocumentClient-interface die de handtekeningen van alle DocumenClient, eigenschappen en methoden definieert toegevoegd.  Als onderdeel van deze wijziging ook gewijzigd uitbreidingsmethoden die IQueryable en IOrderedQueryable methoden van de klasse DocumentClient zelf te maken.
  - Configuratieoptie stelt u de ServicePoint.ConnectionLimit voor een bepaalde DocumentDB eindpunt Uri toegevoegd.  Met ConnectionPolicy.MaxConnectionLimit kunt u de standaardwaarde is ingesteld op 50.
  - Verminderde IPartitionResolver en de uitvoering ervan.  Ondersteuning voor IPartitionResolver is nu verouderd. Het verdient aanbeveling gebruik te maken van verzamelingen gepartitioneerd voor hogere opslag en doorvoer.


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - Toegevoegd dat executestoredprocedureasync methode die RequestOptions als parameter neemt op basis van een overbelasting voor Uri.
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Toegevoegde tijd live (TTL)-ondersteuning voor documenten.

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - Vaste fout in Nuget verpakking van .NET SDK voor verpakking als onderdeel van een oplossing voor Azure Cloud Service.
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - Geïmplementeerde [gepartitioneerde collecties](documentdb-partition-data.md) en [door de gebruiker gedefinieerde prestaties](documentdb-performance-levels.md). 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Vast]** DocumentDB eindpunt genereert opvragen: ' System.Net.Http.HttpRequestException: fout bij het kopiëren van inhoud naar een stream.

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Uitgebreide LINQ ondersteunen met inbegrip van nieuwe marktdeelnemers voor wisselbestand, voorwaardelijke expressies en vergelijking variëren.
    - Nemen inschakelen Selecteer boven in het LINQ-operator
    - Operator CompareTo bereik tekenreeksvergelijkingen inschakelen
    - Voorwaardelijk (?) en query-operators (?)
  - **[Vast]** ArgumentOutOfRangeException bij het combineren van projectie met Model waar In in het linq-query.  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Vast]** Als Schakel niet de laatste expressie is de LINQ Provider ervan uitgegaan dat er geen projectie en selecteer geproduceerd * onjuist.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Geïmplementeerde Upsert, methoden van UpsertXXXAsync toegevoegd
 - Verbeteringen voor alle aanvragen
 - Ondersteuning voor LINQ Provider voorwaardelijk, coalesce en CompareTo methoden voor tekenreeksen
 - **[Vast]** LINQ provider methode implementeren bevat in de lijst voor het genereren van de dezelfde SQL als op IEnumerable en matrix-->
 - **[Vast]** BackoffRetryUtility maakt gebruik van de dezelfde HttpRequestMessage opnieuw in plaats van een nieuw op opnieuw maken
 - **[Verouderd]** UriFactory.CreateCollection--> UriFactory.CreateDocumentCollection moet nu gebruiken
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Vast]** Lokalisatie bij gebruik van niet en cultuur info zoals nl-NL enz. 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - Routering op basis van ID
    - Resource-koppelingen op basis van nieuwe UriFactory helper om te helpen bij het maken van ID
    - Nieuwe overbelastingen op DocumentClient te nemen in de URI
  - IsValid() en IsValidDetailed() in LINQ voor georuimtelijke toegevoegd
  - Verbeterde ondersteuning voor LINQ Provider
    - **Wiskunde** - Abs, Acos, Asin, BOOGTAN, maximum, Cos, Exp, vloer, logboek, Log10, Pow, ronde, teken, Sin, WORTEL, Tan, afkappen
    - **String** - Concat, bevat, EndsWith, IndexOf, aantal, ToLower, TrimStart, vervangen, omkeren, TrimEnd, StartsWith, subtekenreeks, ToUpper
    - **Matrix** - Concat, bevat, het aantal
    - Operator **IN**

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Ondersteuning toegevoegd voor de indexing-beleid wijzigen
    - Nieuwe methode van ReplaceDocumentCollectionAsync in DocumentClient
    - Nieuwe eigenschap van de IndexTransformationProgress in ResourceResponse<T> voor het bijhouden van de procentuele voortgang van beleidswijzigingen index
    - DocumentCollection.IndexingPolicy is nu veranderlijke
  - Ondersteuning toegevoegd voor ruimtelijke indexeren en query
    - Nieuwe Microsoft.Azure.Documents.Spatial-naamruimte voor het serialiseren/deserialiseren van ruimtelijke typen, zoals punt en Veelhoek
    - Nieuwe SpatialIndex-klasse voor het indexeren van de gegevens in DocumentDB GeoJSON
  - **[Vast]** : onjuiste SQL-query is gegenereerd op basis van linq expressie [#38](https://github.com/Azure/azure-documentdb-net/issues/38)

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Afhankelijkheid van Newtonsoft.Json v5.0.7 
- Wijzigingen ter ondersteuning van Order By
  - Ondersteuning voor LINQ provider voor OrderBy() of OrderByDescending()
  - IndexingPolicy ter ondersteuning van Order By 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Ondersteuning voor het partitioneren van gegevens met behulp van de nieuwe klassen van HashPartitionResolver en RangePartitionResolver en de IPartitionResolver
- DataContract serialisatie
- GUID-ondersteuning in LINQ provider
- UDF-ondersteuning in LINQ

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- GA SDK

> [AZURE.NOTE]
Er is een wijziging van de pakketnaam NuGet tussen preview en GA. Wij verplaatst van **Microsoft.Azure.Documents.Client** naar **Microsoft.Azure.DocumentDB**
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-Preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- Voorbeeld SDK's [verouderd]

## <a name="release--retirement-dates"></a>Release & pensioen datums
Microsoft biedt mededeling ten minste **12 maanden** voor het intrekken van een SDK om vloeiend de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en optimalisaties worden alleen toegevoegd aan de huidige SDK, als zodanig wordt het aanbevolen dat u altijd een upgrade naar de nieuwste SDK versie zo vroeg mogelijk. 

Elk verzoek tot DocumentDB met een teruggetrokken SDK wordt afgewezen door de service.

> [AZURE.WARNING]
Alle versies van de Azure DocumentDB SDK voor .NET voorafgaand aan versie **1.0.0** wordt ingetrokken op **29 februari 2016**. 
 
<br/>
 
| Versie | Releasedatum | Pensioendatum 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 27 september 2016 |---
| [1.9.5](#1.9.5) | 01 september 2016 |---
| [1.9.4](#1.9.4) | 24 augustus 2016 |---
| [1.9.3](#1.9.3) | 15 augustus 2016 |---
| [1.9.2](#1.9.2) | 23 juli 2016 |---
| 1.9.1 | Vervangen |---
| 1.9.0 | Vervangen |---
| [1.8.0](#1.8.0) | 14 juni 2016 |---
| [1.7.1](#1.7.1) | 06 mei 2016 |---
| [1.7.0](#1.7.0) | 26 april 2016 |---
| [1.6.3](#1.6.3) | 08 april 2016 |---
| [1.6.2](#1.6.2) | 29 maart 2016 |---
| [1.5.3](#1.5.3) | 19 februari 2016 |---
| [1.5.2](#1.5.2) | 14 december 2015 |---
| [1.5.1](#1.5.1) | 23 november 2015 |---
| [1.5.0](#1.5.0) | 05 oktober 2015 |---
| [1.4.1](#1.4.1) | 25 augustus 2015 |---
| [1.4.0](#1.4.0) | 13 augustus 2015 |---
| [1.3.0](#1.3.0) | 05 augustus 2015 |---
| [1.2.0](#1.2.0) | 06 juli 2015 |---
| [1.1.0](#1.1.0) | 30 april 2015 |---
| [1.0.0](#1.0.0) | 08 april 2015 |---
| [0.9.3-prelease](#0.9.x-preview) | 12 maart 2015 | 29 februari 2016
| [0.9.2-prelease](#0.9.x-preview) | Januari 2015 | 29 februari 2016
| [.9.1-prelease](#0.9.x-preview) | 13 oktober 2014 | 29 februari 2016
| [0.9.0-prelease](#0.9.x-preview) | 21 augustus 2014 | 29 februari 2016

## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zie ook

Zie voor meer informatie over DocumentDB, [DocumentDB voor Microsoft Azure](https://azure.microsoft.com/services/documentdb/) servicepagina. 
