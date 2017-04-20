<properties
pageTitle="Veldtoewijzingen in Azure Search indexeerfuncties"
description="Azure Search indexer veldtoewijzingen voor verschillen in veldnamen en gegevensweergaven configureren"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/17/2016"
ms.author="eugenesh" />

# <a name="field-mappings-in-azure-search-indexers"></a>Veldtoewijzingen in Azure Search indexeerfuncties

Wanneer u indexeerfuncties Azure zoeken, kunt u zelf af en toe in situaties waar uw invoergegevens niet helemaal overeen met het schema van de doel-index vinden. In dat geval kunt u **veldtoewijzingen** uw gegevens transformeren naar de gewenste vorm. 

Enkele situaties waarin het veldtoewijzingen handig:
 
- Uw gegevensbron bevat een veld `_id`, maar is niet toegestaan Azure Search namen beginnen met een onderstrepingsteken. De veldtoewijzing van een kunt u een veld 'naam'. 
- Wilt u verschillende velden in de index met dezelfde gegevens gegevens, bijvoorbeeld omdat u wilt verschillende analyzers toepassen die velden te vullen. Veldtoewijzingen kunnen u het veld voor een gegevensbron zich ' splitsen'.
- U moet met Base64 coderen of decoderen van de gegevens. Veldtoewijzingen ondersteunen diverse **functies voor toewijzing van**functies voor Base64 coderen en decoderen.   


> [AZURE.IMPORTANT] Veld toewijzingen functionaliteit is momenteel in de voorvertoning. Het is alleen beschikbaar in de REST API versie **2015-02-28-voorbeeld**. Neem onthouden, voorbeeld van API's zijn bedoeld voor test- en evaluatiedoeleinden en mag niet worden gebruikt in een productieomgeving.

## <a name="setting-up-field-mappings"></a>Veldtoewijzingen instellen

U kunt veldtoewijzingen toevoegen bij het maken van een nieuwe indexering met behulp van de API voor [Indexering maken](search-api-indexers-2015-02-28-preview.md#create-indexer) . U kunt veldtoewijzingen op indexeren met behulp van de API voor [Indexering Update](search-api-indexers-2015-02-28-preview.md#update-indexer) indexeerfunctie beheren. 

Een toewijzing bestaat uit 3 onderdelen: 

1. A `sourceFieldName`, waarmee een veld in de gegevensbron voorstelt. Deze eigenschap is vereist. 

2. Een optionele `targetFieldName`, die staat voor een veld in de zoekindex. Als dit argument wordt weggelaten, gebruikt dezelfde naam als de gegevensbron in. 

3. Een optionele `mappingFunction`, die kan transformeren uw gegevens met behulp van een van de vooraf gedefinieerde functies. De volledige lijst van functies is [hieronder](#mappingFunctions).

Toewijzingen velden worden toegevoegd aan de `fieldMappings` matrix op de definitie van de indexeerfunctie. 

Hier is hoe u verschillen in veldnamen kan bevatten: 

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
} 
```

Een indexeerfunctie kan meerdere veldtoewijzingen hebben. Hier volgt een voorbeeld van hoe u kunt 'zich splitsen"een veld:

```JSON

"fieldMappings" : [ 
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }, 
] 
```

> [AZURE.NOTE] Azure Search wordt niet hoofdlettergevoelig vergelijking kunt oplossen door het veld en de functie namen in het veldtoewijzingen. Dit is handig (u hoeft niet direct alle behuizing), maar het betekent dat uw gegevensbron of index kan geen velden die alleen per geval verschillen.  

<a name="mappingFunctions"></a>
## <a name="field-mapping-functions"></a>Veld toewijzing van functies

Deze functies worden momenteel ondersteund: 

- [base64Encode](#base64EncodeFunction)
- [base64Decode](#base64DecodeFunction)
- [extractTokenAtPosition](#extractTokenAtPositionFunction)
- [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>
### <a name="base64encode"></a>base64Encode 

Hiermee voert u *URL-safe* Base64-codering van de invoerreeks. Wordt aangenomen dat de invoer UTF-8 gecodeerd. 

#### <a name="sample-use-case"></a>Monster use-case 

Alleen veilige URL tekens kunnen worden weergegeven in een document zoeken Azure sleutel (omdat klanten moeten kunnen om het document met de API voor het zoeken, bijvoorbeeld). Gebruik deze functie als u uw gegevens veilig met URL tekens bevat en u wilt gebruiken voor het vullen van een veld in de zoekindex.   

#### <a name="example"></a>Voorbeeld 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Path", 
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" } 
  }] 
```

<a name="base64DecodeFunction"></a>
### <a name="base64decode"></a>base64Decode

Voert het decoderen van de invoerreeks Base64. De invoer wordt uitgegaan van een *URL-safe* Base64 gecodeerde tekenreeks. 

#### <a name="sample-use-case"></a>Monster use-case 

BLOB aangepaste metagegevenswaarden zijn ASCII-codering. U kunt Base64-codering voor het weergeven van willekeurige Unicode-tekenreeksen in de blob-aangepaste metagegevens. Echter als u wilt zoeken betekenisvolle, kunt u deze functie om de gecodeerde gegevens terug in 'gewone' tekenreeksen tijdens het vullen van de zoekindex.  

#### <a name="example"></a>Voorbeeld 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Base64EncodedMetadata", 
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" } 
  }] 
```

<a name="extractTokenAtPositionFunction"></a>
### <a name="extracttokenatposition"></a>extractTokenAtPosition

Een veld met het opgegeven scheidingsteken wordt gesplitst en hervat het token op de opgegeven positie in de resulterende splitsing.

Als de invoer is bijvoorbeeld `Jane Doe`, de `delimiter` is `" "`(spatie) en de `position` is 0 en het resultaat is `Jane`; Als de `position` 1, is het resultaat is `Doe`. Als de positie naar een token dat niet bestaat verwijst, wordt een fout geretourneerd.

#### <a name="sample-use-case"></a>Monster use-case 

Uw gegevensbron bevat een `PersonName` veld en u wilt deze index als twee aparte `FirstName` en `LastName` velden. Deze functie kunt u de invoer met spaties als scheidingsteken splitsen.

#### <a name="parameters"></a>Parameters

- `delimiter`: een tekenreeks als scheidingsteken wordt gebruikt wanneer de invoerreeks splitsen.
- `position`: een geheel getal op nul gebaseerde positie van het token te kiezen als de invoerreeks is gesplitst.    

#### <a name="example"></a>Voorbeeld

```JSON 

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } } 
  }, 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } } 
  }] 
```

<a name="jsonArrayToStringCollectionFunction"></a>
### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection

Zet een tekenreeks die is opgemaakt als een JSON-matrix van tekenreeksen in een string-matrix die kan worden gebruikt voor het vullen van een `Collection(Edm.String)` veld in de index. 

Als de invoerreeks is bijvoorbeeld `["red", "white", "blue"]`, vervolgens het veld met het doel van het type `Collection(Edm.String)` wordt gevuld met de waarden van de drie `red`, `white` en `blue`. Voor kunnen niet worden geparseerd als JSON tekenreeksmatrices invoerwaarden, wordt een fout geretourneerd. 

#### <a name="sample-use-case"></a>Monster use-case

Azure SQL database geen ingebouwd gegevenstype die natuurlijk aan `Collection(Edm.String)` velden in Azure zoeken. String collectievelden worden gevuld, uw brongegevens als een tekenreeksmatrix JSON opmaken en deze functie wilt gebruiken. 

#### <a name="example"></a>Voorbeeld 

```JSON

"fieldMappings" : [ 
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
] 
```

## <a name="help-us-make-azure-search-better"></a>Help ons verbeteren Azure zoeken

Als u hebt de functie aanvragen of ideeën voor verbeteringen, neem bereiken ons op onze [site UserVoice](https://feedback.azure.com/forums/263029-azure-search/).