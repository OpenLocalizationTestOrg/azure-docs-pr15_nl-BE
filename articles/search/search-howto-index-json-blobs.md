<properties
pageTitle="JSON BLOB's met Azure Search blob indexeerfunctie indexeren"
description="JSON BLOB's met Azure Search blob indexeerfunctie indexeren"
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
ms.date="07/26/2016"
ms.author="eugenesh" />

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>JSON BLOB's met Azure Search blob indexeerfunctie indexeren 

In dit artikel ziet u hoe zoeken Azure blob indexeerfunctie als BLOB's met JSON gestructureerde inhoud te configureren.

## <a name="scenarios"></a>Scenario 's

Standaard parseert [Zoeken Azure blob indexeerfunctie](search-howto-indexing-azure-blob-storage.md) JSON BLOB's als een enkel deel van de tekst. Vaak wilt u de structuur van een JSON-documenten behouden. Bijvoorbeeld, gegeven de JSON-document 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

u kunt het parseren tot een document Azure zoeken met 'tekst', 'datePublished' en 'tags' velden.

Wanneer de BLOB's een **array van objecten JSON bevatten**, kunt u ook elk element van de matrix te worden van een afzonderlijk document voor Azure Search. Bijvoorbeeld een blob met deze JSON gegeven:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

u kunt uw Azure zoekindex met 3 afzonderlijke documenten, elk met een 'id' en 'tekst' op te vullen. 

> [AZURE.IMPORTANT] Deze functionaliteit is momenteel in de voorvertoning. Het is alleen beschikbaar in de REST API versie **2015-02-28-voorbeeld**. Neem onthouden, voorbeeld van API's zijn bedoeld voor test- en evaluatiedoeleinden en mag niet worden gebruikt in een productieomgeving. 

## <a name="setting-up-json-indexing"></a>JSON indexering instellen

JSON BLOB's indexeren, stelt u de `parsingMode` configuratieparameter `json` (voor elke blob als één document index) of `jsonArray` (als de BLOB's een JSON-matrix bevatten): 

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

Gebruik indien noodzakelijk **veldtoewijzingen** te kiezen, de eigenschappen van het brondocument JSON gebruikt voor het vullen van de doel-zoekindex.  Dit wordt hieronder uitvoerig beschreven. 

> [AZURE.IMPORTANT] Als u werkt met `json` of `jsonArray` modus parseren, Azure Search wordt ervan uitgegaan dat alle BLOB's in uw gegevensbron JSON worden. Als u nodig hebt ter ondersteuning van een mix van JSON en niet-JSON BLOB's in dezelfde gegevensbron, laat het ons weten op [onze website UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="using-field-mappings-to-build-search-documents"></a>Met behulp van veldtoewijzingen te bouwen documenten zoeken 

Op dit moment Azure Search kan niet worden geïndexeerd willekeurige JSON-documenten rechtstreeks, omdat het ondersteunt alleen primitieve gegevenstypen tekenreeksmatrices en GeoJSON punten. U kunt echter **veldtoewijzingen** picken onderdelen van een JSON-document en 'tilt' ze in de bovenste velden van het zoekdocument. Zie informatie over het veld toewijzingen basisbeginselen, [Azure Search indexer veldtoewijzingen de verschillen tussen gegevensbronnen en zoekindexen overbruggen](search-indexer-field-mappings.md).

Terugkomen in ons voorbeeld JSON-document: 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Stel dat u een zoekindex met de volgende velden hebben: `text` van het type Edm.String, `date` van het type Edm.DateTimeOffset, en `tags` van het type Collection(Edm.String). Als u wilt uw JSON toewijzen in de gewenste vorm, gebruik de volgende veldtoewijzingen: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
    ]

De veldnamen van de bron van de toewijzingen worden opgegeven met de [Aanwijzer JSON](http://tools.ietf.org/html/rfc6901) -notatie. U begint met een slash om te verwijzen naar de hoofdmap van uw JSON-document en vervolgens met behulp van voorwaartse slash gescheiden pad de gewenste eigenschap (op willekeurige niveau van nesten) lager. 

U kunt ook afzonderlijke matrixelementen verwijzen met behulp van een op nul gebaseerde index. Bijvoorbeeld, om te selecteren op het eerste element van de matrix 'tags' uit het bovenstaande voorbeeld, gebruiken een toewijzing als volgt:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Als de naam van een bron in een veld toewijzing pad naar een eigenschap die niet in JSON verwijst, toewijzing overgeslagen zonder een fout. Dit wordt gedaan zodat we kunnen documenten met een ander schema (dit is een algemene use-case) ondersteunen. Omdat er geen validatie, moet u de nodige waakzaamheid om te voorkomen dat typefouten in het veld toewijzing specificatie. 

Als de JSON-documenten alleen eenvoudige eigenschappen van het hoogste niveau bevatten, moet u wellicht niet veldtoewijzingen helemaal. Bijvoorbeeld als uw JSON uitziet, de eigenschappen op het hoogste niveau 'tekst', 'datePublished' en 'tags' rechtstreeks toegewezen aan de overeenkomstige velden in de zoekindex: 
 
    { 
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
    }

## <a name="indexing-nested-json-arrays"></a>Geneste matrices van JSON indexeren

Wat gebeurt er als u wilt een array van objecten JSON, maar die array index zich ergens in het document? U kunt kiezen welke eigenschap bevat de matrix met behulp van de `documentRoot` configuratie-eigenschap. Als bijvoorbeeld de BLOB's zien er als volgt: 

    { 
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" }, 
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    } 

de configuratie voor de index van de matrix in de eigenschap '2' gebruiken: 

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Voorbeelden van aanvraag

Dit alle plaatsen, Hier vindt u de volledige payloads voorbeelden. 

Gegevensbron: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Indexeerfunctie:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]
    }

## <a name="help-us-make-azure-search-better"></a>Help ons verbeteren Azure zoeken

Als u hebt de functie aanvragen of ideeën voor verbeteringen, neem bereiken ons op onze [site UserVoice](https://feedback.azure.com/forums/263029-azure-search/).