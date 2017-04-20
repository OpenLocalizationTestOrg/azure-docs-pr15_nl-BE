
<properties
    pageTitle="DocumentDB Java API en SDK | Microsoft Azure"
    description="Meer weten over de Java API en SDK, inclusief release datums, pensioen, datums en wijzigingen die zijn aangebracht tussen elke versie van de Java SDK, DocumentDB."
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-API's en SDK 's

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>DocumentDB Java API en SDK

<table>
<tr><td>**SDK downloaden**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**API-documentatie**</td><td>[Java API-documentatie](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Bijdragen tot de SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Aan de slag**</td><td>[Aan de slag met de SDK voor Java](documentdb-java-application.md)</td></tr>
<tr><td>**Huidige ondersteunde runtime**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Release-opmerkingen

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - Ondersteuning toegevoegd voor een consistent niveau van BoundedStaleness.
  - Ondersteuning toegevoegd voor rechtstreekse connectiviteit voor CRUD bewerkingen voor de gepartitioneerde collecties.
  - Vast een bug in een SQL-database opvragen.
  - Vast een bug in de sessiecache van de waarin sessietoken is misschien onjuist ingesteld.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - Ondersteuning toegevoegd voor cross-partitie parallelle query's.
  - Ondersteuning toegevoegd voor TOP/ORDER BY-query's voor de gepartitioneerde collecties.
  - Ondersteuning toegevoegd voor een sterke samenhang.
  - Ondersteuning toegevoegd voor op basis van de naam aanvragen bij het gebruik van directe verbinding.
  - Vaste ActivityId consistent blijven in alle aanvraagpogingen te maken.
  - Vast een bug die betrekking hebben op de sessiecache wanneer een collectie met dezelfde naam opnieuw te maken.
  - Veelhoek en toegevoegd LineString DataTypes collectie indexing beleid voor ruimtelijke geo-hekwerk-query's te specificeren.
  - Vaste problemen met Java Doc voor Java 1,8.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - Vast een bug in PartitionKeyDefinitionMap met één partitie collecties in de cache en extra ophalen sleutel aanvragen partitie te maken.
  - Vast een bug niet opnieuw als een belangrijke waarde onjuist partitie beschikbaar is.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - De ondersteuning voor meerdere regio database accounts toegevoegd.
  - Ondersteuning toegevoegd voor automatische opnieuw op beperkt aanvragen met opties voor het aanpassen van het maximum aantal nieuwe pogingen en de maximale wachttijd.  Zie RetryOptions en ConnectionPolicy.getRetryOptions().
  - IPartitionResolver afgeschaft op basis van aangepaste partitionering code. Gebruik de gepartitioneerde collecties voor hogere opslag en doorvoer.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- Toegevoegde opnieuw Beleidsondersteuning voor bandbreedteregeling.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Toegevoegde tijd live (TTL)-ondersteuning voor documenten.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Geïmplementeerde [gepartitioneerde collecties](documentdb-partition-data.md) en [door de gebruiker gedefinieerde prestaties](documentdb-performance-levels.md).

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- Vast een bug in HashPartitionResolver voor het genereren van hashwaarden in overeenstemming te zijn met andere SDK's little-endian.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Hash- & bereik toevoegen partitie resolvers om te helpen bij het sharding toepassingen op verschillende partities.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Upsert implementeren. Nieuwe upsertXXX-methoden toegevoegd ter ondersteuning van de functie Upsert.
- Op basis van id-routering geïmplementeerd. Er zijn geen openbare API wijzigingen, alle interne wijzigingen.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- Versie versienummer brengen via andere SDK's overgeslagen

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Ondersteunt georuimtelijke Index
- De eigenschap id voor alle resources valideert. Id's voor bronnen mag niet ?, /, #, \, tekens of eindigen met een spatie.
- Nieuwe kop "index transformatie uitvoering" toegevoegd aan ResourceResponse.

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- V2 indexing beleid implementeert

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- GA SDK

## <a name="release--retirement-dates"></a>Release & pensioen datums
Microsoft biedt mededeling ten minste **12 maanden** voor het intrekken van een SDK om vloeiend de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en optimalisaties worden alleen toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade naar de nieuwste SDK versie zo vroeg mogelijk.

Elk verzoek tot DocumentDB met een teruggetrokken SDK wordt afgewezen door de service.

> [AZURE.WARNING]
Alle versies van de Azure DocumentDB SDK voor Java dan versie **1.0.0** wordt ingetrokken op **29 februari 2016**.

<br/>

| Versie | Releasedatum | Pensioendatum
| ---     | ---          | ---
| [1.9.1](#1.9.1) | 28 oktober 2016 |---
| [1.9.0](#1.9.0) | 03 oktober 2016 |---
| [1.8.1](#1.8.1) | 30 juni 2016 |---
| [1.8.0](#1.8.0) | 14 juni 2016 |---
| [1.7.1](#1.7.1) | 30 april 2016 |---
| [1.7.0](#1.7.0) | 27 april 2016 |---
| [1.6.0](#1.6.0) | 29 maart 2016 |---
| [1.5.1](#1.5.1) | 31 december 2015 |---
| [1.5.0](#1.5.0) | 04 december 2015 |---
| [1.4.0](#1.4.0) | 05 oktober 2015 |---
| [1.3.0](#1.3.0) | 05 oktober 2015 |---
| [1.2.0](#1.2.0) | 05 augustus 2015 |---
| [1.1.0](#1.1.0) | 09 juli 2015 |---
| [1.0.1](#1.0.1) | 12 mei 2015 |---
| [1.0.0](#1.0.0) | 07 april 2015 |---
| 0.9.5-prelease | 09 mrt 2015 | 29 februari 2016
| 0.9.4-prelease | 17 februari 2015 | 29 februari 2016
| 0.9.3-prelease | 13 januari 2015 | 29 februari 2016
| 0.9.2-prelease | 19 december 2014 | 29 februari 2016
| 0.9.1-prelease | 19 december 2014 | 29 februari 2016
| 0.9.0-prelease | 10 december 2014 | 29 februari 2016

## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zie ook

Zie voor meer informatie over DocumentDB, [DocumentDB voor Microsoft Azure](https://azure.microsoft.com/services/documentdb/) servicepagina.
