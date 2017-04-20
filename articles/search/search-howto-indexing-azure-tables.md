<properties
pageTitle="Azure tabelopslag met Azure Search Indexing"
description="Meer informatie over het indexeren van de gegevens die zijn opgeslagen in tabellen met Azure Search Azure"
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
ms.date="08/16/2016"
ms.author="eugenesh" />

# <a name="indexing-azure-table-storage-with-azure-search"></a>Azure tabelopslag met Azure Search Indexing

In dit artikel ziet u hoe met Azure Search indexgegevens opgeslagen in de tabelopslag van Azure. De nieuwe tabel voor indexering Azure Search gaat dit snel en naadloos. 

> [AZURE.IMPORTANT] Deze functionaliteit is momenteel in de voorvertoning. Het is alleen beschikbaar in de REST API versie **2015-02-28-voorbeeld** en in versie 2.0-voorbeeld van de SDK voor .NET. Neem onthouden, voorbeeld van API's zijn bedoeld voor test- en evaluatiedoeleinden en mag niet worden gebruikt in een productieomgeving.

## <a name="setting-up-azure-table-indexing"></a>Indexering van Azure tabel instellen

Als u wilt instellen en configureren van een indexeerfunctie Azure tabel, kunt u de Azure Search REST API maken en beheren van **indexeerfuncties** en **gegevensbronnen** , zoals beschreven in de [indexeerfunctie bewerkingen](https://msdn.microsoft.com/library/azure/dn946891.aspx). Ook kunt u [versie 2.0-voorbeeld](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) van de SDK voor .NET. In de toekomst ondersteuning voor het indexeren van de tabel wordt toegevoegd aan de Portal Azure.

Een gegevensbron geeft aan welke gegevens aan de index, referenties die nodig zijn voor toegang tot de gegevens, en het beleid waarmee Azure Search efficiënt wijzigingen in de gegevens (nieuw, gewijzigd of verwijderde rijen) te identificeren.

Indexeerfunctie leest gegevens uit een gegevensbron en laadt deze in de zoekindex van een doel.

Indexering van de tabel instellen:

1. Een gegevensbron maken
    - Stel de `type` parameter`azuretable`
    - Doorgegeven in de verbindingsreeks van de opslag-account als de `credentials.connectionString` parameter
    - Geef de tabel naam met behulp van de `container.name` parameter
    - Geef desgewenst ook een query met de `container.query` parameter. Gebruik waar mogelijk een filter op een PartitionKey voor de beste prestaties; een andere query resulteert in een volledige tabelscan, wat leiden slechte prestaties voor grote tabellen tot kan.
2. Maak een zoekindex met het schema dat overeenkomt met de kolommen in de tabel die u wilt indexeren. 
3. De indexeerfunctie maken door de gegevensbron verbinden aan de zoekindex.

### <a name="create-data-source"></a>Gegevensbron maken

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Zie voor meer informatie over de gegevensbron maken API [Gegevensbron maken](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="create-index"></a>Index maken 

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
        ]
    }

Zie [Index maken](https://msdn.microsoft.com/library/dn798941.aspx) voor meer informatie over de API van de Index maken

### <a name="create-indexer"></a>Indexeerfunctie maken 

Tot slot maakt de indexeerfunctie die verwijst naar de gegevensbron en de doel-index. Bijvoorbeeld:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Bekijk voor meer informatie over de API voor indexering maken [Maken voor indexering](search-api-indexers-2015-02-28-preview.md#create-indexer).

Dat is alles klaar is Kees - fijne indexing!

## <a name="dealing-with-different-field-names"></a>Omgaan met verschillende namen

De veldnamen in de bestaande index worden vaak verschillen van de namen van de eigenschappen in de tabel. **Veldtoewijzingen** kunt u de namen van de eigenschappen van de tabel met de veldnamen in de zoekindex toewijzen. Zie voor meer informatie over het toewijzen van een veld, de [Azure Search indexer veldtoewijzingen de verschillen tussen gegevensbronnen en zoekindexen overbruggen](search-indexer-field-mappings.md).

## <a name="handling-document-keys"></a>Toetsen voor document verwerken

In Azure zoeken, de documentsleutel is uniek voor een document. Elke zoekindex moet precies één veld van het type `Edm.String`. Het sleutelveld is vereist voor elk document dat wordt toegevoegd aan de index (in feite is het enige verplichte veld).

Omdat het een samengestelde sleutel tabelrijen, Azure Search genereert een synthetische veld met de naam `Key` is een samenvoeging van de partitie-toets en de rij sleutelwaarden. Bijvoorbeeld, als een rij van het PartitionKey is `PK1` en RowKey `RK1`, vervolgens `Key` de waarde van het veld `PK1RK1`. 

> [AZURE.NOTE] De `Key` waarde bevat mogelijk tekens die in het document toetsen, zoals streepjes ongeldig zijn. U kunt omgaan met ongeldige tekens met behulp van de `base64Encode` [veld toewijzing functie](search-indexer-field-mappings.md#base64EncodeFunction). Als u dit doet, moet u ook gebruik van URL-safe Base64-codering als document sleutels worden doorgegeven in de API zoals opzoeken aanroept.

## <a name="incremental-indexing-and-deletion-detection"></a>Detectie van incrementele indexering en verwijdering
 
Bij het instellen van een tabel voor indexering uitvoeren volgens een schema dat alleen nieuwe of bijgewerkte rijen, afhankelijk van de rij van de reindexes `Timestamp` waarde. U hoeft niet te geven van een beleid voor wijziging detectie – incrementele indexering wordt u automatisch ingeschakeld. 

Toevoegen om aan te geven dat bepaalde documenten moeten worden verwijderd uit de index, kunt u een zachte verwijderen strategie – in plaats van het verwijderen van een rij, een eigenschap om aan te geven dat deze is verwijderd en een zachte verwijdering detectie beleid op de gegevensbron instellen. Het beleid dat hieronder wordt bijvoorbeeld dat een rij wordt verwijderd als er een eigenschap `IsDeleted` met de waarde `"true"`: 

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]
    
    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "query" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   


## <a name="help-us-make-azure-search-better"></a>Help ons verbeteren Azure zoeken

Als u hebt de functie aanvragen of ideeën voor verbeteringen, neem bereiken ons op onze [site UserVoice](https://feedback.azure.com/forums/263029-azure-search/).