<properties
    pageTitle="Het vak connector toevoegt aan uw logica Apps | Microsoft Azure"
    description="Overzicht van de verbindingslijn vak met REST API parameters"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-box-connector"></a>Aan de slag met het vak connector
Verbinding maken met het vak en maken van bestanden en bestanden verwijderen. 

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld.

U kunt met het vak:

- Bouw uw bedrijf flow op basis van de gegevens die u in het vak. 
- Triggers gebruiken wanneer een bestand wordt gemaakt of bijgewerkt.
- Acties die een bestand kopiëren, verwijderen van een bestand, en nog veel meer gebruiken. Deze acties een reactie krijgt en vervolgens de uitvoer beschikbaar maken voor andere acties. Bijvoorbeeld, wanneer een bestand is gewijzigd in een vak, kunt u dat bestand en e-mailen met Office 365.

Zie als u wilt een bewerking in apps logica toevoegen, [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties
Vak bevat de volgende trigger en acties.

| Triggers | Acties|
| --- | --- |
|<ul><li>Wanneer een bestand wordt gemaakt</li><li>Wanneer een bestand wordt gewijzigd</li></ul> | <ul><li>Bestand maken</li><li>Wanneer een bestand wordt gemaakt</li><li>Bestand kopiëren</li><li>Bestand verwijderen</li><li>Archief map uitpakken</li><li>Inhoud van het bestand met behulp van id ophalen</li><li>Inhoud van bestand met behulp van pad</li><li>Metagegevens van het bestand met behulp van id ophalen</li><li>Ophalen van metagegevens van het bestand met behulp van pad</li><li>Update-bestand</li><li>Wanneer een bestand wordt gewijzigd</li></ul>

Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens.

## <a name="create-a-connection-to-box"></a>Een verbinding maken met het vak
Wanneer u deze connector aan uw apps logica toevoegen, moet u de logica apps verbinding maken met uw vak machtigen.

>[AZURE.INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]

Nadat u de verbinding hebt gemaakt, kunt u de eigenschappen invoeren. De **REST API reference** in dit onderwerp worden deze eigenschappen beschreven.

>[AZURE.TIP] In andere logica apps kunt u deze verbinding met hetzelfde vak.

## <a name="swagger-rest-api-reference"></a>Swagger REST API: naslag
Van toepassing op versie: 1.0.

### <a name="create-file"></a>Bestand maken
Een bestand wordt geüpload naar vak.  
```POST: /datasets/default/files```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|folderPath|tekenreeks|Ja|query|Geen |Pad naar het bestand te uploaden naar vak|
|naam|tekenreeks|Ja|query|Geen |Naam van het bestand maken in het vak|
|hoofdtekst|String(Binary) |Ja|hoofdtekst|Geen |Inhoud van het bestand te uploaden naar vak|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="when-a-file-is-created"></a>Wanneer een bestand wordt gemaakt
Een stroom wordt gegenereerd wanneer een nieuw bestand wordt gemaakt in een map van het vak.  
```GET: /datasets/default/triggers/onnewfile```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|map-id|tekenreeks|Ja|query|Geen |De unieke id van de map in het vak|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="copy-file"></a>Bestand kopiëren
Een bestand kopiëren naar het vak.  
```POST: /datasets/default/copyFile```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|bron|tekenreeks|Ja|query|Geen |URL van bestand|
|bestemming|tekenreeks|Ja|query| Geen|Bestemming het pad in het vak, met inbegrip van de doelbestandsnaam|
|overschrijven|Boole-waarde|Nee|query| Geen|Overschrijft het doelbestand als is ingesteld op 'true'|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="delete-file"></a>Bestand verwijderen
Verwijdert een bestand uit het vak.  
```DELETE: /datasets/default/files/{id}```


| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|Geen |De unieke id van het bestand dat u wilt verwijderen uit het vak|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="extract-archive-to-folder"></a>Archief map uitpakken
Haalt u een gearchiveerd bestand in een map in het vak (voorbeeld: .zip).  
```POST: /datasets/default/extractFolderV2```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|bron|tekenreeks|Ja|query| |Pad naar het bestand|
|bestemming|tekenreeks|Ja|query| |Pad in het vak om de inhoud van het archief te pakken|
|overschrijven|Boole-waarde|Nee|query| |De gekopieerde bestanden worden overschreven als is ingesteld op 'true'|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="get-file-content-using-id"></a>Inhoud van het bestand met behulp van id ophalen
Bestandsinhoud opgehaald van het kiezen van id.  
```GET: /datasets/default/files/{id}/content```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|Geen |De unieke id van het bestand in het vak|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="get-file-content-using-path"></a>Inhoud van bestand met behulp van pad
Bestandsinhoud wordt opgehaald uit het vak pad.  
```GET: /datasets/default/GetFileContentByPath```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|pad|tekenreeks|Ja|query|Geen |Unieke pad naar het bestand in het vak|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="get-file-metadata-using-id"></a>Metagegevens van het bestand met behulp van id ophalen
Metagegevens van bestanden van het vak met de bestands-id opgehaald.  
```GET: /datasets/default/files/{id}```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad| Geen|De unieke id van het bestand in het vak|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="get-file-metadata-using-path"></a>Ophalen van metagegevens van het bestand met behulp van pad
Metagegevens van bestanden opgehaald uit het vak pad.  
```GET: /datasets/default/GetFileByPath```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|pad|tekenreeks|Ja|query|Geen |Unieke pad naar het bestand in het vak|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="update-file"></a>Update-bestand
Een bestand in het vak wordt bijgewerkt.  
```PUT: /datasets/default/files/{id}```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad| Geen|De unieke id van het bestand bij te werken in het vak|
|hoofdtekst|String(Binary) |Ja|hoofdtekst|Geen |Inhoud van het bestand bij te werken in het vak|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="when-a-file-is-modified"></a>Wanneer een bestand wordt gewijzigd
Een stroom wordt gegenereerd wanneer een bestand is gewijzigd in een vak map.  
```GET: /datasets/default/triggers/onupdatedfile```

| Naam|Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|map-id|tekenreeks|Ja|query|Geen |De unieke id van de map in het vak|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities

#### <a name="datasetsmetadata"></a>DataSetsMetadata

|Naam van eigenschap | Gegevenstype | Vereist|
|---|---|---|
|in tabelvorm|niet gedefinieerd|geen|
|BLOB|niet gedefinieerd|geen|

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|bron|tekenreeks|geen|
|displayName|tekenreeks|geen|
|urlEncoding|tekenreeks|geen|
|tableDisplayName|tekenreeks|geen|
|tablePluralName|tekenreeks|geen|

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|bron|tekenreeks|geen|
|displayName|tekenreeks|geen|
|urlEncoding|tekenreeks|geen|

#### <a name="blobmetadata"></a>BlobMetadata

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|Naam|tekenreeks|geen|
|DisplayName|tekenreeks|geen|
|Pad|tekenreeks|geen|
|LastModified|tekenreeks|geen|
|Grootte|geheel getal|geen|
|MediaType|tekenreeks|geen|
|IsFolder|Boole-waarde|geen|
|ETag|tekenreeks|geen|
|FileLocator|tekenreeks|geen|

## <a name="next-steps"></a>Volgende stappen

[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).
