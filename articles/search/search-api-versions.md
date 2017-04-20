<properties
   pageTitle="API-versies van Azure Search | Microsoft Azure | Zoek-API"
   description="Het beleid voor Azure Search REST API's en de clientbibliotheek in de SDK voor .NET versie."
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/16/2016"
   ms.author="brjohnst"/>

# <a name="api-versions-in-azure-search"></a>API-versies in Azure zoeken

Azure Search rolt van functie-updates op regelmatige basis. Soms, maar niet altijd, deze updates moet u ons voor het publiceren van een nieuwe versie van onze API om compatibiliteit te behouden. Publiceren van een nieuwe versie, kunt u bepalen wanneer en hoe u search service updates in uw code integreren.

We proberen in de regel voor het publiceren van nieuwe versies alleen indien nodig, omdat deze mogelijk moeite uw code voor het gebruik van een nieuwe API-versie bijwerkt. Wij zullen een nieuwe versie alleen publiceren als moeten we bepaalde aspecten van de API op een manier die achterwaartse compatibiliteit te veranderen. Dit kan gebeuren door verbeteringen aan bestaande functies of door nieuwe functies die het oppervlak van de bestaande API wijzigen.

We volgen dezelfde regel voor updates van SDK. De SDK Azure Search volgt de regels [semantische versioning](http://semver.org/) , wat betekent dat de versie die bestaat uit drie delen: primaire, secundaire en build-nummer (bijvoorbeeld 1.1.0). We brengen een nieuwe primaire versie van de SDK alleen in het geval van wijzigingen die problemen met achterwaartse compatibiliteit. Voor vaste functie-updates, we de secundaire versie wordt verhoogd en voor het oplossen van programmafouten we alleen de build-versie wordt vergroot.

##<a name="snapshot-of-current-versions"></a>Momentopname van de huidige versies 

Hieronder wordt een momentopname van de huidige versies weergegeven van alle programmeerinterfaces Azure zoeken. Routekaarten en andere details vindt u in de volgende secties van dit document.

Interfaces|Laatste primaire versie|Status
----------|-------------------------|------
[.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)|1.1|In het algemeen beschikbaar, uitgebracht in februari 2016
[Voorbeeld van de .NET SDK](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx)|2.0-voorbeeld|Voorbeeld van augustus 2016 vrijgegeven
[REST API-service](https://msdn.microsoft.com/library/azure/dn798935.aspx)|2015-02-28|In het algemeen beschikbaar
[Service REST API voorbeeld](search-api-2015-02-28-preview.md)|2015-02-28-voorbeeld|Voorbeeld
[REST API voor beheer](https://msdn.microsoft.com/library/azure/dn832684.aspx)|2015-08-19|In het algemeen beschikbaar

Voor de REST API's, met inbegrip van de `api-version` op elke oproep is vereist. Dit kunt u gemakkelijk om een specifieke versie, zoals een voorbeeld-API. In het volgende voorbeeld ziet u hoe de `api-version` -parameter is opgegeven:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2015-02-28

> [AZURE.NOTE] Hoewel elke aanvraag heeft een `api-version`, wordt aangeraden dat u dezelfde versie voor alle API-aanvragen gebruiken. Dit geldt met name wanneer nieuwe versies van de API introduceren kenmerken of bewerkingen die niet worden herkend door eerdere versies. Mengen van API-versies hebben onbedoelde gevolgen en moeten worden vermeden.
> 
De REST API-Service en de REST-API voor beheer zijn versioned onafhankelijk van elkaar. Elke gelijkenis versienummers is collega incidentele schade.

In het algemeen beschikbaar (of GA) API's kunnen worden gebruikt in de productie en Azure serviceovereenkomsten vallen. Preview-versies hebben experimentele functies die niet altijd worden gemigreerd naar een versie van NH. **We raden u ten zeerste aan met API's voorvertoning in productietoepassingen.**

##<a name="sdk-version-roadmap"></a>SDK versie roadmap

Elke versie van de .NET SDK is bedoeld voor een bepaalde versie van de Service REST API. Functies zijn eerst uitgerold in de REST API en vervolgens in de SDK worden geïmplementeerd.

De .NET SDK is nu algemeen beschikbaar en zijn al bezig met de volgende versie. In de volgende tabel zoekt vooruit voor toekomstige versies van de SDK, zodat u een idee van wat hierna.

Versie van de SDK voor .NET|REST API versie|Functies|ETA
----------------|----------------|--------|---
1.1|2015-02-28|Lucene query-syntaxis|Februari 2016
2.0-voorbeeld|2015-02-28-voorbeeld|Aangepaste analyzers, Azure Blob en tabel indexeerfuncties, veldtoewijzingen, ETags|Augustus 2016
2.x|Nieuwe versie van de API van NH|Zelfde als 2.0-voorbeeld|Vroege Q4 2016

##<a name="about-preview-and-generally-available-versions"></a>Over afdrukvoorbeeld en algemeen verkrijgbare versies

Azure Search releases altijd vooraf experimentele functies via de REST API eerst, vervolgens door middel van voorlopige versies van de SDK voor .NET.

Voorbeeld-functies zijn niet gegarandeerd worden gemigreerd naar een versie van NH. Overwegende dat functies in een NH-versie worden beschouwd als stabiel en waarschijnlijk niet te wijzigen, met uitzondering van kleine neerwaarts compatibele correcties en uitbreidingen, zijn voorbeeld functies beschikbaar voor het testen en experimenteren met het doel van het verzamelen van feedback op het functionele ontwerp en uitvoering. 

Echter aangezien voorbeeld functies kunnen gewijzigd worden, is het raadzaam tegen schrijven productiecode waarmee een afhankelijkheid in de preview-versies. Als u een oudere voorbeeldversie gebruikt, wordt u aangeraden migreren naar de algemeen beschikbare (GA) versie. 

Voor de .NET SDK: richtlijnen voor de migratie van de code kunt u vinden op [de .NET SDK Upgrade](search-dotnet-sdk-migration.md).

Algemene beschikbaarheid betekent Azure zoeken is nu onder de serviceovereenkomst (SLA). De SLA kan worden gevonden op [Azure Search Service Level Agreements](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

