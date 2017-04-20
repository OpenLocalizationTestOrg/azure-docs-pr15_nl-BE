<properties 
    pageTitle="DocumentDB Python API en SDK | Microsoft Azure" 
    description="Meer weten over de Python API en SDK, inclusief release datums, pensioen, datums en wijzigingen die zijn aangebracht tussen elke versie van de Python DocumentDB SDK." 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-API's en SDK 's

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>DocumentDB Python API en SDK

<table>
<tr><td>**SDK downloaden**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**API-documentatie**</td><td>[Python API-documentatie](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**SDK-installatie-instructies**</td><td>[Installatie-instructies voor Python SDK](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Bijdragen tot de SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Aan de slag**</td><td>[Aan de slag met de SDK Python](documentdb-python-application.md)</td></tr>
<tr><td>**Huidige ondersteund platform**</td><td>[Python 2.7](https://www.python.org/downloads/) en [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Release-opmerkingen

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- Toegevoegde ondersteuning voor Python 3.5.
- Ondersteuning toegevoegd voor het groeperen van verbindingen met een module aanvragen.
- Ondersteuning toegevoegd voor de consistentie van de sessie.
- Ondersteuning toegevoegd voor TOP/sorteren op query's voor de gepartitioneerde collecties.


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Ondersteuning voor Groepsbeleid opnieuw toegevoegd voor beperkt aanvragen. (Beperkt aanvragen ontvangen een verzoek tarief te groot uitzondering, foutcode 429). Standaard DocumentDB pogingen negen-maal voor elke aanvraag als foutcode 429 wordt aangetroffen, de tijd van de retryAfter in de response-header behouden blijft. Een vaste opnieuw intervaltijd kan nu worden ingesteld als onderdeel van de eigenschap RetryOptions van het ConnectionPolicy-object als u wilt dat de tijd van de retryAfter geretourneerd door server tussen de pogingen wordt genegeerd. DocumentDB nu wacht maximaal 30 seconden een aanvraag die verminderd (onafhankelijk van het aantal nieuwe pogingen) en retourneert het antwoord met foutcode 429. Deze tijd kan ook worden overschreven in de eigenschap RetryOptions op ConnectionPolicy-object.

- DocumentDB retourneert nu x-ms-throttle-aantal nieuwe pogingen en x-ms-throttle-retry-wait-time-ms, zoals de antwoordheaders in elke aanvraag om aan te geven van de gasklep opnieuw tellen en de cummulative de aanvraag worden gewacht tussen de pogingen.

- De klasse RetryPolicy en de bijbehorende eigenschap (retry_policy) zichtbaar op de klasse document_client verwijderd en in plaats daarvan een RetryOptions klasse blootstelling van de eigenschap RetryOptions van de klasse ConnectionPolicy die kan worden gebruikt voor het overschrijven van enkele van de opties standaard opnieuw geïntroduceerd.

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - De ondersteuning voor meerdere regio database accounts toegevoegd.

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- De ondersteuning voor de functie van de tijd-Live(TTL) voor documenten toegevoegd.

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- Correcties voor het server-side partitioneren om speciale tekens in het pad partitionkey.

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- Geïmplementeerde [gepartitioneerde collecties](documentdb-partition-data.md) en [door de gebruiker gedefinieerde prestaties](documentdb-performance-levels.md). 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Hash- & bereik toevoegen partitie resolvers om te helpen bij het sharding toepassingen op verschillende partities.

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Upsert implementeren. Nieuwe UpsertXXX-methoden toegevoegd ter ondersteuning van de functie Upsert.
- Op basis van id-routering geïmplementeerd. Er zijn geen openbare API wijzigingen, alle interne wijzigingen.

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Ondersteunt georuimtelijke index.
- De eigenschap id voor alle resources valideert. Id's voor bronnen mag niet ?, /, #, \, tekens of eindigen met een spatie.
- Nieuwe kop "index transformatie uitvoering" toegevoegd aan ResourceResponse.

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- V2 indexing beleid implementeert.

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Proxyverbinding ondersteunt.

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA SDK.

## <a name="release--retirement-dates"></a>Datums release & pensioen
Microsoft biedt mededeling ten minste **12 maanden** voor het intrekken van een SDK om vloeiend de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en optimalisaties worden alleen toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade naar de nieuwste SDK versie zo vroeg mogelijk. 

Elk verzoek tot DocumentDB met een teruggetrokken SDK wordt afgewezen door de service.

> [AZURE.WARNING]
Alle versies van de SDK Azure DocumentDB voor Python dan versie **1.0.0** wordt ingetrokken op **29 februari 2016**. 

<br/>

| Versie | Releasedatum | Pensioendatum 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | 29 september 2016 |---
| [1.9.0](#1.9.0) | 07 juli 2016 |---
| [1.8.0](#1.8.0) | 14 juni 2016 |---
| [1.7.0](#1.7.0) | 26 april 2016 |---
| [1.6.1](#1.6.1) | 08 april 2016 |---
| [1.6.0](#1.6.0) | 29 maart 2016 |---
| [1.5.0](#1.5.0) | 03 januari 2016 |---
| [1.4.2](#1.4.2) | 06 oktober 2015 |---
| [1.4.1](#1.4.1) | 06 oktober 2015 |---
| [1.2.0](#1.2.0) | 06 augustus 2015 |---
| [1.1.0](#1.1.0) | 09 juli 2015 |---
| [1.0.1](#1.0.1) | 25 mei 2015 |---
| [1.0.0](#1.0.0) | 07 april 2015 |---
| 0.9.4-prelease | 14 januari 2015 | 29 februari 2016
| 0.9.3-prelease | 09 december 2014 | 29 februari 2016
| 0.9.2-prelease | 25 november 2014 | 29 februari 2016
| 0.9.1-prelease | 23 september 2014 | 29 februari 2016
| 0.9.0-prelease | 21 augustus 2014 | 29 februari 2016

## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zie ook

Zie voor meer informatie over DocumentDB, [DocumentDB voor Microsoft Azure](https://azure.microsoft.com/services/documentdb/) servicepagina. 
