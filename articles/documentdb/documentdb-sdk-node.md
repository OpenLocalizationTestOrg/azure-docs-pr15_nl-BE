<properties
    pageTitle="DocumentDB Node.js API en SDK | Microsoft Azure"
    description="Meer weten over de Node.js API en SDK, inclusief release datums, pensioen, datums en wijzigingen die zijn aangebracht tussen elke versie van de SDK DocumentDB Node.js."
    services="documentdb"
    documentationCenter="nodejs"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-API's en SDK 's

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##<a name="documentdb-nodejs-api-and-sdk"></a>DocumentDB Node.js API en SDK

<table>
<tr><td>**SDK downloaden**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**API-documentatie**</td><td>[Node.js API-documentatie](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**SDK-installatie-instructies**</td><td>[Installatie-instructies](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**Bijdragen tot de SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**Voorbeelden**</td><td>[Codevoorbeelden node.js](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**Get begonnen zelfstudie**</td><td>[Aan de slag met de SDK Node.js](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Zelfstudie op het web app**</td><td>[Bouwen van een webtoepassing voor Node.js met DocumentDB](documentdb-nodejs-application.md)</td></tr>
<tr><td>**Huidige ondersteund platform**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##<a name="release-notes"></a>Release-opmerkingen

###<a name="1.10.0"/>1.10.0</a>

- Ondersteuning toegevoegd voor cross-partitie parallelle query's.
- Ondersteuning toegevoegd voor TOP/ORDER BY-query's voor de gepartitioneerde collecties.

###<a name="1.9.0"/>1.9.0</a>

- Ondersteuning voor Groepsbeleid opnieuw toegevoegd voor beperkt aanvragen. (Beperkt aanvragen ontvangen een verzoek tarief te groot uitzondering, foutcode 429). Standaard DocumentDB pogingen negen-maal voor elke aanvraag als foutcode 429 wordt aangetroffen, de tijd van de retryAfter in de response-header behouden blijft. Een vaste opnieuw intervaltijd kan nu worden ingesteld als onderdeel van de eigenschap RetryOptions van het ConnectionPolicy-object als u wilt dat de tijd van de retryAfter geretourneerd door server tussen de pogingen wordt genegeerd. DocumentDB nu wacht maximaal 30 seconden een aanvraag die verminderd (onafhankelijk van het aantal nieuwe pogingen) en retourneert het antwoord met foutcode 429. Deze tijd kan ook worden overschreven in de eigenschap RetryOptions op ConnectionPolicy-object.

- DocumentDB retourneert nu x-ms-throttle-aantal nieuwe pogingen en x-ms-throttle-retry-wait-time-ms, zoals de antwoordheaders in elke aanvraag om aan te geven van de gasklep opnieuw tellen en de cummulative de aanvraag worden gewacht tussen de pogingen.

- De klasse RetryOptions is toegevoegd, de eigenschap RetryOptions op de ConnectionPolicy-klasse die kan worden gebruikt voor het overschrijven van enkele van de opties standaard opnieuw bloot.

###<a name="1.8.0"/>1.8.0</a>

 - De ondersteuning voor meerdere regio database accounts toegevoegd.

###<a name="1.7.0"/>1.7.0</a>

- De ondersteuning voor de functie van de tijd-Live(TTL) voor documenten toegevoegd.

###<a name="1.6.0"/>1.6.0</a>

- Geïmplementeerde [gepartitioneerde collecties](documentdb-partition-data.md) en [door de gebruiker gedefinieerde prestaties](documentdb-performance-levels.md).

###<a name="1.5.6"/>1.5.6 worden uitgezet</a>

- Vaste RangePartitionResolver.resolveForRead bug waar koppelingen als gevolg van een slechte concat van resultaten is niet retourneren.

###<a name="1.5.5"/>1.5.5</a>

- Vaste hashParitionResolver resolveForRead(): wanneer geen partitiesleutel opgegeven uitzondering, in plaats van een lijst van alle geregistreerde koppelingen is genereren.

###<a name="1.5.4"/>1.5.4</a>

- Correcties afgifte [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - HTTPS-Agent speciaal: Vermijd het wijzigen van de globale agent voor DocumentDB doeleinden. Gebruik een speciale agent voor alle aanvragen van de lib.

###<a name="1.5.3"/>1.5.3</a>

- Correcties [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - correct ingang streepjes in de media-ID's worden verleend.

###<a name="1.5.2"/>1.5.2</a>

- Correcties uitgeven [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - listener lekken waarschuwing EventEmitter.

###<a name="1.5.1"/>1.5.1</a>

- Correcties uitgeven [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - map hernoemen Hash met de hash voor systemen met hoofdletters en kleine letters.

### <a name="1.5.0"/>1.5.0</a>

- Sharding biedt ondersteuning door bereik & Hash-partitie resolvers toe te voegen.

### <a name="1.4.0"/>1.4.0</a>

- Upsert implementeren. Nieuwe methoden voor upsertXXX op documentClient.

### <a name="1.3.0"/>1.3.0</a>

- Om de versienummers in uitlijnen met andere SDK's overgeslagen.

### <a name="1.2.2"/>1.2.2</a>

- Gesplitste Q belooft wrapper nieuwe opslagplaats.
- Bijwerken naar pakketbestand voor npm-register.

### <a name="1.2.1"/>1.2.1</a>

- Implements-ID op basis van de routering.
- Opgelost probleem [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - huidige eigenschap veroorzaakt een conflict met de methode current().

### <a name="1.2.0"/>1.2.0</a>

- Ondersteuning toegevoegd voor georuimtelijke index.
- De eigenschap id voor alle resources valideert. Kunnen geen id's voor bronnen bevatten?, /, #, & #47; & #47; tekens of eindigen met een spatie.
- Nieuwe kop "index transformatie uitvoering" toegevoegd aan ResourceResponse.

### <a name="1.1.0"/>1.1.0</a>

- V2 indexing beleid implementeert.

### <a name="1.0.3"/>1.0.3</a>

- Probleem #40 (https://github.com/Azure/azure-documentdb-node/issues/40) - geïmplementeerd eslint en grunt-configuraties in de kern en promise SDK.

### <a name="1.0.2"/>1.0.2</a>

- Probleem [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - wrapper beloften bevat geen header met fout.

### <a name="1.0.1"/>1.0.1</a>

- Geïmplementeerde mogelijkheid om de query voor conflicten door toevoeging van readConflicts, readConflictAsync en queryConflicts.
- Bijgewerkte API-documentatie.
- [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - fout client.createDocumentAsync uitgeven.

### <a name="1.0.0"/>1.0.0</a>

- GA SDK.

## <a name="release--retirement-dates"></a>Release & pensioen datums
Microsoft biedt mededeling ten minste **12 maanden** voor het intrekken van een SDK om vloeiend de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en optimalisaties worden alleen toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade naar de nieuwste SDK versie zo vroeg mogelijk.

Elk verzoek tot DocumentDB met een teruggetrokken SDK wordt afgewezen door de service.

> [AZURE.WARNING]
Alle versies van de SDK Azure DocumentDB voor Node.js dan versie **1.0.0** wordt ingetrokken op **29 februari 2016**.

<br/>

| Versie | Releasedatum | Pensioendatum
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 03 oktober 2016 |---
| [1.9.0](#1.9.0) | 07 juli 2016 |---
| [1.8.0](#1.8.0) | 14 juni 2016 |---
| [1.7.0](#1.7.0) | 26 april 2016 |---
| [1.6.0](#1.6.0) | 29 maart 2016 |---
| [1.5.6 worden uitgezet](#1.5.6) | 08 maart 2016 |---
| [1.5.5](#1.5.5) | 02 februari 2016 |---
| [1.5.4](#1.5.4) | 01 februari 2016 |---
| [1.5.2](#1.5.2) | 26 januari 2016 |---
| [1.5.2](#1.5.2) | 22 januari 2016 |---
| [1.5.1](#1.5.1) | 4 januari 2016 |---
| [1.5.0](#1.5.0) | 31 december 2015 |---
| [1.4.0](#1.4.0) | 06 oktober 2015 |---
| [1.3.0](#1.3.0) | 06 oktober 2015 |---
| [1.2.2](#1.2.2) | 10 september 2015 |---
| [1.2.1](#1.2.1) | 15 augustus 2015 |---
| [1.2.0](#1.2.0) | 05 augustus 2015 |---
| [1.1.0](#1.1.0) | 09 juli 2015 |---
| [1.0.3](#1.0.3) | 04 juni 2015 |---
| [1.0.2](#1.0.2) | 23 mei 2015 |---
| [1.0.1](#1.0.1) | 15 mei 2015 |---
| [1.0.0](#1.0.0) | 08 april 2015 |---
| 0.9.4-prerelease | 06 april 2015 | 29 februari 2016
| 0.9.3-prerelease | 14 januari 2015 | 29 februari 2016
| 0.9.2-prerelease | 18 december 2014 | 29 februari 2016
| 0.9.1-prerelease | 22 augustus 2014 | 29 februari 2016
| 0.9.0-prerelease | 21 augustus 2014 | 29 februari 2016


## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zie ook

Zie voor meer informatie over DocumentDB, [DocumentDB voor Microsoft Azure](https://azure.microsoft.com/services/documentdb/) servicepagina.
