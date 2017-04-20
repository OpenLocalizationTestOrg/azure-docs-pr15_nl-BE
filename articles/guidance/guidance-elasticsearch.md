
<properties
   pageTitle="Elasticsearch in Azure richtlijnen | Microsoft Azure"
   description="Elasticsearch in Azure richtlijnen."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="elasticsearch-on-azure-guidance"></a>Elasticsearch in Azure richtlijnen 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch is een zeer schaalbare open source zoekmachine en de database. Het is handig voor situaties waarin snelle analyse en het ontdekken van informatie in grote datasets. Deze richtsnoeren wordt gekeken naar enkele belangrijke aspecten te overwegen bij het ontwerpen van een cluster Elasticsearch met een focus op manieren te optimaliseren en uw configuratie testen.

> [AZURE.NOTE]Deze instructies wordt ervan uitgegaan dat de enige elementaire kennis van Elasticsearch. Bezoek de [website van Elasticsearch](https://www.elastic.co/products/elasticsearch)voor meer gedetailleerde informatie. 

- **[Elasticsearch uitgevoerd op Azure][]** biedt een korte inleiding tot de algemene structuur van Elasticsearch en wordt beschreven hoe u een Elasticsearch cluster met Azure te implementeren. 
- **[Afstemmen gegevens ingestie prestaties voor Elasticsearch op Azure][]** vindt de implementatie van een cluster Elasticsearch en diepgaande analyse van de verschillende configuratieopties wanneer u verwacht dat een hoge frequentie van de opname van de gegevens.
- **[Afstemmen het samenvoegen van gegevens en de prestaties van query's voor Elasticsearch op Azure][]** biedt een diepgaande analyse van de opties rekening houden als het optimaliseren van uw systeem voor query-en besluiten.
- Beschrijving van enkele belangrijke functies van een cluster Elasticsearch die kunnen helpen bij het minimaliseren van de kans op verlies van gegevens en uitgebreide data recovery tijden **[veerkracht en recovery op Elasticsearch op Azure configureren][]** .
- **[Maken van een testomgeving prestaties voor Elasticsearch op Azure][]** wordt beschreven hoe u een omgeving voor het testen van de prestaties gegevens via ingestie en de werkbelasting van de query in een cluster Elasticsearch instellen. 
- **[Implementatie van een JMeter testen plan voor Elasticsearch][]** geeft een overzicht van lopende prestatietests geïmplementeerd met JMeter testplannen samen met de code van Java-JUnit test voor het uitvoeren van taken, zoals het uploaden van gegevens in het cluster Elasticsearch opgenomen.
- Beschrijving van het maken en gebruiken van een JUnit sampler die kunt genereren en uploaden van gegevens naar een cluster Elasticsearch **[implementeren een sampler JMeter-JUnit Elasticsearch prestaties testen][]** . Dit biedt een zeer flexibele benadering voor het laden van testen die grote hoeveelheden testgegevens zonder afhankelijk van externe gegevensbestanden kunt genereren. 
- **[De geautomatiseerde Elasticsearch resiliency tests uitgevoerd][]** ziet u hoe u het testen van tolerantie in deze reeks worden gebruikt. 
- **[De geautomatiseerde Elasticsearch prestatietests uitvoeren][]** geeft een overzicht van het uitvoeren van de prestatietests in deze reeks worden gebruikt.


[Elasticsearch uitgevoerd op Azure]: guidance-elasticsearch-running-on-azure.md
[Optimaliseren gegevens ingestiedosiscoëfficiënten voor Elasticsearch op Azure]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Prestatietest omgeving voor Elasticsearch op Azure maken]: guidance-elasticsearch-creating-performance-testing-environment.md
[Implementatie van een testplan JMeter voor Elasticsearch]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[Implementatie van een Sampler JMeter-JUnit Elasticsearch prestaties testen]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Het samenvoegen van gegevens en de prestaties van query's voor Elasticsearch op Azure afstemmen]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[Veerkracht en herstel configureren op Elasticsearch op Azure]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[De tolerantie voor geautomatiseerde Elasticsearch Tests uitgevoerd]: guidance-elasticsearch-running-automated-resilience-tests.md
[De geautomatiseerde Elasticsearch prestatietests uitvoeren]: guidance-elasticsearch-running-automated-performance-tests.md
