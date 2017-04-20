<properties
pageTitle="Azure Blob-opslag met Azure Search Indexing"
description="Informatie over het indexeren van Azure Blob-opslag en tekst ophalen uit documenten met Azure Search"
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
ms.date="10/16/2016"
ms.author="eugenesh" />

# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Documenten in Azure Blob-opslag met Azure Search Indexing

In dit artikel ziet u hoe u met Azure Search index documenten (zoals PDF's, Microsoft Office-documenten en diverse andere algemene indelingen) opgeslagen in Azure Blob-opslag. De nieuwe zoekopdracht Azure blob indexeerfunctie gaat dit snel en naadloos. 

> [AZURE.IMPORTANT] Deze functionaliteit is momenteel in de voorvertoning. Het is alleen beschikbaar in de REST API versie **2015-02-28-voorbeeld**. Neem onthouden, voorbeeld van API's zijn bedoeld voor test- en evaluatiedoeleinden en mag niet worden gebruikt in een productieomgeving.

## <a name="supported-document-formats"></a>Ondersteunde documentindelingen voor

De indexeerfunctie blob kunt u tekst ophalen uit de volgende documentindelingen:

- PDF-BESTAND
- Microsoft Office-indelingen: DOCX/DOC, XLSX/XLS, PPTX/PPT, MSG (Outlook e-mails)  
- HTML-CODE
- XML
- ZIP
- EML
- Tekstbestanden zonder opmaak  
- JSON (Zie [Indexing JSON BLOB's](search-howto-index-json-blobs.md) voor details)
- CSV (Zie [Indexing CSV-BLOB's](search-howto-index-csv-blobs.md) voor details)

## <a name="setting-up-blob-indexing"></a>Blob indexering instellen

U kunt instellen dat een Azure Blob-opslag voor indexering gebruiken:

- [Azure portal](https://ms.portal.azure.com)
- Azure Search [REST API](https://msdn.microsoft.com/library/azure/dn946891.aspx)
- Azure Search .NET SDK [versie 2.0-voorbeeld](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) 

> [AZURE.NOTE] Sommige functies (bijvoorbeeld veldtoewijzingen) zijn niet beschikbaar in de portal en via een programma worden gebruikt. 

In dit artikel, zult u stelt indexeerfunctie REST API gebruiken door de volgende drie stappen: Maak een gegevensbron, een index maken, de indexering configureren.

### <a name="step-1-create-a-data-source"></a>Stap 1: Een gegevensbron maken

Een gegevensbron geeft aan welke gegevens aan de index, referenties die nodig zijn voor toegang tot de gegevens en het beleid voor het efficiënt identificeren wijzigingen in de gegevens (nieuwe, gewijzigde of verwijderde rijen). Een gegevensbron kan worden gebruikt door meerdere indexeerfuncties in dezelfde directory zoeken.

De gegevensbron moet de volgende eigenschappen vereist zijn voor het indexeren van de blob: 

- **is de unieke naam van de gegevensbron in de search-service.** 

- **type** moet `azureblob`. 

- **referenties** biedt de verbindingsreeks van opslag account als de `credentials.connectionString` parameter. Krijgt u de verbindingsreeks van de Azure portal door te navigeren naar de gewenste opslag account blade > **Instellingen** > **-sleutels** en gebruik de "Primaire verbindingsreeks" of "Secundaire verbindingsreeks" waarde. 

- **container** geeft een container in uw account voor opslag. Standaard zijn alle BLOB's in de container worden opgehaald. Als u alleen index BLOB's in een bepaalde virtuele map, kunt u deze map met behulp van de optionele **query** -parameter. 

In het volgende voorbeeld ziet u een definitie van de gegevensbron:

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Zie voor meer informatie over de gegevensbron maken API [Gegevensbron maken](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="step-2-create-an-index"></a>Stap 2: Een index maken 

De index bevat de velden in een document, kenmerken, en andere constructies die de vorm van de zoekopdracht ervaring.  

Blob indexeren, moet u ervoor zorgen dat de index een doorzoekbare is `content` veld voor het opslaan van de blob.

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
        ]
    }

Zie [Index maken](https://msdn.microsoft.com/library/dn798941.aspx) voor meer informatie over de API van de Index maken

### <a name="step-3-create-an-indexer"></a>Stap 3: Maak een indexeerfunctie 

Een indexeerfunctie gegevensbronnen met doel zoekindexen verbindt en planningsinformatie biedt, zodat u de gegevens vernieuwen kunt automatiseren. Als de index en de gegevensbron zijn gemaakt, is het relatief eenvoudig te maken die verwijst naar de gegevensbron en een index doel indexeerfunctie. Bijvoorbeeld:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Deze indexering wordt uitgevoerd om de twee uur (schema-interval is ingesteld op 'PT2H'). Indexeerfunctie uitvoeren instellen elke 30 minuten het interval op 'PT30M'. Kortste ondersteunde interval is 5 minuten. Planning is optioneel - als dit argument wordt weggelaten, indexeerfunctie wordt slechts eenmaal uitgevoerd wanneer gemaakt. U kunt echter voor indexering op aanvraag uitvoeren op elk gewenst moment.   

Bekijk voor meer informatie over de API voor indexering maken [Maken voor indexering](search-api-indexers-2015-02-28-preview.md#create-indexer).


## <a name="document-extraction-process"></a>Document ophaalproces

Azure Search indexeert elk document (blob) als volgt:

- De inhoud van de volledige tekst van het document wordt geëxtraheerd in een veld met de naam `content`. Wij bieden momenteel niet ondersteuning voor het extraheren van meerdere documenten uit een enkele blob:
    - Bijvoorbeeld, wordt een CSV-bestand geïndexeerd als één document. Als u elke regel in een CSV als een apart document worden behandeld, stem op [deze suggestie UserVoice](https://feedback.azure.com/forums/263029-azure-search/suggestions/13865325-please-treat-each-line-in-a-csv-file-as-a-separate).
    - Een samengestelde of ingesloten document (zoals een ZIP-archief of een Word-document met ingesloten Outlook e-mailbericht met bijlagen) wordt ook als één document geïndexeerd.

- Metagegevenseigenschappen van gebruiker opgegeven aanwezig zijn op de blob, worden indien van toepassing, geëxtraheerd woordelijk. De eigenschappen van metagegevens kunnen ook worden gebruikt om bepaalde aspecten van het proces van de extractie – [Met behulp van aangepaste metagegevens van het Document extractie](#CustomMetadataControl) Zie voor meer informatie.

- Eigenschappen van metagegevens standaard blob worden uitgepakt in de volgende velden:

    - **metagegevens\_opslag\_naam** (Edm.String) - de naam van de blob. Als er een blob-/my-container/my-folder/subfolder/resume.pdf, de waarde van dit veld is bijvoorbeeld `resume.pdf`.

    - **metagegevens\_opslag\_pad** (Edm.String) - de volledige URI van de blob, met inbegrip van de opslag-account. Bijvoorbeeld:`https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`

    - **metagegevens\_opslag\_inhoud\_type** (Edm.String) - type zoals aangegeven door de code die u gebruikt voor het uploaden van de blob inhoud. Bijvoorbeeld `application/octet-stream`.

    - **metagegevens\_opslag\_laatste\_aangepast** (Edm.DateTimeOffset) - tijdstempel voor de blob het laatst is gewijzigd. Azure Search wordt dit tijdstempel gewijzigde BLOB's, om te voorkomen dat alles na de eerste indexering opnieuw te indexeren.

    - **metagegevens\_opslag\_grootte** (Edm.Int64) - blob-grootte in bytes.

    - **metagegevens\_opslag\_inhoud\_md5** (Edm.String) - MD5-hash van de blob inhoud, indien beschikbaar.

- Eigenschappen van metagegevens specifiek voor elke indeling worden uitgepakt in de lijst [hier](#ContentSpecificMetadata).

U hoeft niet voor alle bovenstaande eigenschappen definiëren in de zoekindex - alleen de eigenschappen die u nodig hebt voor uw toepassing opnemen. 

> [AZURE.NOTE] De veldnamen in de bestaande index worden vaak verschillen van de veldnamen die zijn gegenereerd tijdens de extractie van document. **Veldtoewijzingen** kunt u de namen van de eigenschappen die aan de veldnamen in de zoekindex Azure Search verstrekt toewijzen. Ziet u een voorbeeld van het veld toewijzingen hieronder gebruiken. 

## <a name="picking-the-document-key-field-and-dealing-with-different-field-names"></a>Het sleutelveld document verzamelen en omgaan met verschillende namen

In Azure zoeken, de documentsleutel is uniek voor een document. Elke zoekindex moet precies één veld van het type Edm.String hebben. Het sleutelveld is vereist voor elk document dat wordt toegevoegd aan de index (het is eigenlijk het enige verplichte veld).  
   
U moet zorgvuldig overwegen welke uitgepakte veld moet worden toegewezen aan het sleutelveld voor de index. De kandidaten zijn:

- **metagegevens\_opslag\_naam** - dit is mogelijk een geschikte kandidaat, maar de opmerking dat 1) de namen niet uniek zijn mogelijk, zoals u BLOB's met dezelfde naam in verschillende mappen en 2) de naam wellicht bevat mogelijk tekens die in het document toetsen, zoals streepjes ongeldig zijn. U kunt omgaan met ongeldige tekens met behulp van de `base64Encode` , [veldtoewijzing functie](search-indexer-field-mappings.md#base64EncodeFunction) als u dit doet, moet u sleutels document coderen wanneer deze worden doorgegeven in de API zoals opzoeken aanroept. (Bijvoorbeeld in .NET kunt u de [methode UrlTokenEncode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) voor dat doel).

- **metagegevens\_opslag\_pad** - zorgt voor Uniekheid van het volledige pad, maar het pad bevat wel degelijk `/` tekens worden [gebruikt in een documentsleutel](https://msdn.microsoft.com/library/azure/dn857353.aspx).  Zoals hierboven, hebt u de optie codering met sleutels van de `base64Encode` [functie](search-indexer-field-mappings.md#base64EncodeFunction).

- Als geen van de bovenstaande opties voor u werkt, kunt u een eigenschap van het aangepaste metagegevens toevoegen aan de BLOB's. Deze optie is, echter het uploadproces blob die metagegevenseigenschap toevoegen aan alle BLOB's vereist. Omdat de sleutel een vereiste eigenschap is, niet alle BLOB's waarop die eigenschap niet worden geïndexeerd.

> [AZURE.IMPORTANT] Als er geen expliciete toewijzing voor het sleutelveld in de index, Azure zoeken automatisch gebruikt `metadata_storage_path` als sleutel en Base64-gecodeerd sleutelwaarden (de tweede optie hierboven).

In dit voorbeeld gaan we kiezen de `metadata_storage_name` veld als de documentsleutel. We gaan uit van de index is een sleutelveld met de naam `key` en `fileSize` voor de grootte van het document op te slaan. Om dingen wire als gewenst, de volgende veldtoewijzingen bij het maken of bijwerken van de indexeerfunctie geeft:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Om dit allemaal bij elkaar staan, is het veldtoewijzingen toevoegen en base-64 codering van sleutels voor een bestaande indexeerfunctie:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [AZURE.NOTE] Zie voor meer informatie over het toewijzen van een veld, [in dit artikel](search-indexer-field-mappings.md).

## <a name="incremental-indexing-and-deletion-detection"></a>Detectie van incrementele indexering en verwijdering

Bij het instellen van een blob voor indexering uitvoeren op een schema, het opnieuw indexeren van alleen de gewijzigde BLOB's, zoals bepaald door de blob van `LastModified` tijdstempel.

> [AZURE.NOTE] U hoeft niet te geven van een beleid voor wijziging detectie – incrementele indexering wordt u automatisch ingeschakeld.

Gebruik een 'soft delete'-benadering voor het verwijderen van documenten. Als u de rechtstreekse BLOB's verwijdert, worden overeenkomstige documenten niet uit de zoekindex verwijderd. In plaats daarvan gebruikt u de volgende stappen uit:  

1. Een eigenschap van het aangepaste metagegevens toevoegen aan de blob om aan te geven naar Azure Search logisch wordt verwijderd
2. Een zachte verwijdering detectie-beleid configureren voor de gegevensbron
3. Zodra de indexeerfunctie heeft verwerkt de blob (zoals weergegeven door de indexeerfunctie status API), kunt u de blob fysiek verwijderen

Bijvoorbeeld het volgende beleid een blob worden verwijderd als er een metagegevenseigenschap acht `IsDeleted` met de waarde `true`:

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",   
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Eigenschappen van metagegevens van type-specifieke inhoud

In de volgende tabel verwerking vindt voor elke documentindeling bevat een overzicht en beschrijving van de eigenschappen van metagegevens opgehaald door Azure Search.

Indeling / type inhoud | Eigenschappen van bepaalde metagegevens inhoudstype | Verwerking details
-------------------------------|-------------------------------------------|-------------------
HTML (`text/html`) | `metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` | HTML-opmaakcodes verwijderen en extraheren van tekst
PDF (`application/pdf`) | `metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title`| Tekst, met inbegrip van de ingesloten documenten (met uitzondering van afbeeldingen)
DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Tekst, met inbegrip van de ingesloten documenten ophalen
DOC (application/msword) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Tekst, met inbegrip van de ingesloten documenten ophalen
XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Tekst, met inbegrip van de ingesloten documenten ophalen
XLS (application/vnd.ms-excel) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Tekst, met inbegrip van de ingesloten documenten ophalen
PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Tekst, met inbegrip van de ingesloten documenten ophalen
PPT (application/vnd.ms-powerpoint) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Tekst, met inbegrip van de ingesloten documenten ophalen
MSG (application/vnd.ms-outlook) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` | Tekst, inclusief bijlagen ophalen
ZIP (toepassing/zip) | `metadata_content_type` | Tekst ophalen uit alle documenten in het archief
XML application/xml) | `metadata_content_type`</br>`metadata_content_encoding`</br> | XML-opmaakcodes verwijderen en extraheren van tekst
JSON (application/json) | `metadata_content_type`</br>`metadata_content_encoding` | Tekst ophalen<br/>Opmerking: Als u meerdere documentvelden in een blob JSON uitpakken, Zie [Indexing JSON BLOB's](search-howto-index-json-blobs.md) voor meer informatie
EML (message/rfc822) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` | Tekst, inclusief bijlagen ophalen
Tekst zonder opmaak (tekst/plain) | `metadata_content_type`</br>`metadata_content_encoding`</br> | 

<a name="CustomMetadataControl"></a>
## <a name="using-custom-metadata-to-control-document-extraction"></a>Met behulp van aangepaste metagegevens document extractie van besturingselement

U kunt de eigenschappen van metagegevens toevoegen aan een blob bepaalde aspecten van het ophaalproces blob indexeren en het document te bepalen. De volgende eigenschappen worden momenteel ondersteund:

Naam van eigenschap | Eigenschapswaarde | Uitleg
--------------|----------------|------------
AzureSearch_Skip | "true" | Hiermee geeft u de indexeerfunctie blob volledig kunt overslaan de blob; Extractie van metadata noch inhoud wordt geprobeerd. Dit is handig wanneer u wilt overslaan van bepaalde typen inhoud, of wanneer een bepaalde blob mislukt herhaaldelijk en onderbreekt het indexeringsproces.
AzureSearch_SkipContent | "true" | Hiermee geeft u de indexeerfunctie blob index alleen de metagegevens en uitpakken van de inhoud van de blob overslaan. Dit is handig als de inhoud van de blob niet interessant is, maar toch wilt indexeren de metagegevens die zijn gekoppeld aan de blob.

<a name="IndexerParametersConfigurationControl"></a>
## <a name="using-indexer-parameters-to-control-document-extraction"></a>Met behulp van parameters voor indexering extractie document bepalen

Verschillende parameters zijn beschikbaar om te bepalen welke BLOB's en welke delen van de inhoud en de metagegevens van een blob-indexer configuratie worden geïndexeerd. 

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Alleen de BLOB's met specifieke extensies indexeren

U kunt de index alleen BLOB's met de bestandsnaamextensies die u met behulp van opgeeft de `indexedFileNameExtensions` configuratieparameter indexeerfunctie. De waarde is een tekenreeks met een door komma's gescheiden lijst met bestandsextensies (met een voorafgaande punt). Bijvoorbeeld aan de index alleen de. PDF en. DOCX-blobs doen dit: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions-from-indexing"></a>BLOB's met bepaalde bestandsextensies uitsluiten van indexering

U kunt BLOB's met bepaalde bestandsextensies uitsluiten van indexeren met behulp van de `excludedFileNameExtensions` configuratieparameter. De waarde is een tekenreeks met een door komma's gescheiden lijst met bestandsextensies (met een voorafgaande punt). Bijvoorbeeld naar de index alle BLOB's, behalve de pakketten met de. PNG en. JPEG-extensies, doen dit: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Als beide `indexedFileNameExtensions` en `excludedFileNameExtensions` parameters zijn aanwezig, Azure eerst wordt gezocht op `indexedFileNameExtensions`, klik op `excludedFileNameExtensions`. Dit betekent dat als u dezelfde bestandsextensie aanwezig is in beide lijsten, het bestandstype wordt uitgesloten van indexering. 

### <a name="index-storage-metadata-only"></a>Opslag metagegevens van de index alleen

U kunt de metagegevens van de opslag indexeren en volledig overslaan document extractie proces gebruikt de `indexStorageMetadataOnly` configuratie-eigenschap. Dit is handig wanneer u hoeft niet de inhoud van het document, moet u de metagegevens voor content type-specifieke eigenschappen. Hiertoe stelt u de `indexStorageMetadataOnly` eigenschap `true`: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexStorageMetadataOnly" : true } }
    }

### <a name="index-both-storage-and-content-type-metadata-but-skip-content-extraction"></a>Opslag- en metagegevens van het type inhoud te indexeren, maar inhoud extractie overslaan

Als u moet alle metagegevens uitpakken maar inhoud extractie van alle BLOB's overslaan, vraagt u om dit probleem met de configuratie indexer hoeft toe te voegen `AzureSearch_SkipContent` metagegevens voor elk afzonderlijk blob. Hiertoe stelt u de `skipContent` indexeerfunctie configuratie-eigenschap om te `true`: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "skipContent" : true } }
    }

## <a name="help-us-make-azure-search-better"></a>Help ons verbeteren Azure zoeken

Als u hebt de functie aanvragen of ideeën voor verbeteringen, neem bereiken ons op onze [site UserVoice](https://feedback.azure.com/forums/263029-azure-search/).
