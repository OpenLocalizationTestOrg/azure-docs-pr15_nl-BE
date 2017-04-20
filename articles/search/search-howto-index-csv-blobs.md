<properties
pageTitle="CSV-BLOB's met Azure Search blob indexeerfunctie indexeren | Microsoft Azure"
description="Meer informatie over het CSV-BLOB's met Azure Search index"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="07/12/2016"
ms.author="eugenesh" />

# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>CSV-BLOB's met Azure Search blob indexeerfunctie indexeren 

Standaard [Zoeken Azure blob indexeerfunctie](search-howto-indexing-azure-blob-storage.md) parseert gescheiden tekst BLOB's als een enkel deel van de tekst. Echter met blobs met CSV-gegevens, wilt u meestal aan elke regel in de blob als een apart document worden behandeld. Bijvoorbeeld de volgende tekst met scheidingstekens gegeven: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

u kunt parseren in documenten, 2, elk die 'id', 'datePublished' en 'labels'-velden bevatten.

In dit artikel leert u CSV-BLOB's met een blob Azure Search indexer parseren. 

> [AZURE.IMPORTANT] Deze functionaliteit is momenteel in de voorvertoning. Het is alleen beschikbaar in de REST API versie **2015-02-28-voorbeeld**. Neem onthouden, voorbeeld van API's zijn bedoeld voor test- en evaluatiedoeleinden en mag niet worden gebruikt in een productieomgeving. 

## <a name="setting-up-csv-indexing"></a>CSV-indexering instellen

CSV-BLOB's indexeren, maken of bijwerken van een definitie van de indexeerfunctie met de `delimitedText` parseren modus:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Bekijk voor meer informatie over de API voor indexering maken [Maken voor indexering](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`Geeft aan dat de eerste regel (niet leeg) van elke blob kopteksten bevat.
Als BLOB's niet een koptekstregel van de eerste bevatten, moeten de headers worden opgegeven in de configuratie voor indexering: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Op dit moment wordt alleen de UTF-8-codering ondersteund. Ook de door komma's `','` teken als scheidingsteken wordt ondersteund. Als u ondersteuning nodig voor andere coderingen of scheidingstekens, laat het ons weten op [onze website UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [AZURE.IMPORTANT] Wanneer u de tekst met scheidingstekens modus parseren, Azure Search wordt ervan uitgegaan dat alle BLOB's in uw gegevensbron zal CSV. Als u nodig hebt ter ondersteuning van een mix van CSV- en niet-CSV-BLOB's in dezelfde gegevensbron, laat het ons weten op [onze website UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="request-examples"></a>Voorbeelden van aanvraag

Dit alle plaatsen, Hier vindt u de volledige payload voorbeelden. 

Gegevensbron: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexeerfunctie:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Help ons verbeteren Azure zoeken

Als u hebt de functie aanvragen of ideeën voor verbeteringen, neem bereiken ons op onze [site UserVoice](https://feedback.azure.com/forums/263029-azure-search/).