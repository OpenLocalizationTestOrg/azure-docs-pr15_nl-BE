## <a name="specifying-formats"></a>Notaties opgeven

Azure Data Factory ondersteunt de volgende notatietypen: 

- [Tekst zonder opmaak](#specifying-textformat)
- [JSON-indeling](#specifying-jsonformat)
- [Avro-indeling](#specifying-avroformat)
- [ORC-indeling](#specifying-orcformat)
- [Indeling van parketvloeren](#specifying-parquetformat)

### <a name="specifying-textformat"></a>Tekstopmaak opgeven

Als u de indeling op **tekstopmaak**is ingesteld, kunt u de volgende **optionele** eigenschappen in de sectie **Opmaak** .

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------- | -------- | 
| columnDelimiter | Het teken dat wordt gebruikt om de afzonderlijke kolommen in een bestand. | Slechts één teken is toegestaan. De standaardwaarde is een door komma's (','). <br/><br/>Verwijzen naar de [Unicode-tekens](https://en.wikipedia.org/wiki/List_of_Unicode_characters) voor de betreffende code voor het gebruik van een Unicode-teken. Bijvoorbeeld, kunt u overwegen een zeldzame niet-afdrukbare tekens voor die waarschijnlijk niet aanwezig in uw gegevens gebruiken: '\u0001' dat staat voor begin van de kop (SOH) opgeven. | Nee |
| rowDelimiter | Het teken dat wordt gebruikt voor het scheiden van rijen in een bestand. | Slechts één teken is toegestaan. De standaardwaarde is een van de volgende waarden op lezen: ["\r\n", "\r", "\n"] en "\r\n" op schrijven. | Nee |
| escapeChar | Het speciale teken kunt u een scheidingsteken voor kolommen in de inhoud van het invoerbestand gebruikt. <br/><br/>U kunt geen zowel escapeChar als quoteChar voor een tabel. | Slechts één teken is toegestaan. Geen standaardwaarde. <br/><br/>Voorbeeld: als er een komma (', ') als kolomscheidingsteken, maar u wilt de komma teken in de tekst hebt (voorbeeld: "Hello, world"), '$' definiëren als escape-teken en gebruikt de tekenreeks "$Hello, world" in de bron. | Nee | 
| quoteChar | Het teken dat wordt gebruikt om de offerte een string-waarde. De rij- en scheidingstekens binnen de aanhalingstekens zou worden beschouwd als onderdeel van de string-waarde. Deze eigenschap is van toepassing op de invoer en uitvoer datasets.<br/><br/>U kunt geen zowel escapeChar als quoteChar voor een tabel. | Slechts één teken is toegestaan. Geen standaardwaarde. <br/><br/>Bijvoorbeeld, als u een komma hebt (', ') als kolomscheidingsteken, maar u wilt komma in de tekst (voorbeeld: < Hello, world >), kunt u ' (dubbel aanhalingsteken) als het aanhalingsteken en gebruik de tekenreeks "Hello, world" in de bron. | Nee |
| nullValue | Een of meer tekens gebruikt om aan te geven een null-waarde. | Een of meer tekens. De standaardwaarden zijn "\N" en "NULL" op lezen en "\N" op schrijven. | Nee |
| encodingName | Geef de naam van de codering. | Een geldige naam van codering. Zie [De eigenschap Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx). Voorbeeld: windows-1250 of shift_jis. De standaardwaarde is UTF-8. | Nee | 
| firstRowAsHeader | Geeft aan of rekening te houden met de eerste rij als koptekst. Voor een invoer dataset leest Data Factory eerste rij als koptekst. Voor een dataset uitvoer schrijft Data Factory eerste rij als koptekst. <br/><br/>Zie [scenario's voor het gebruik van **firstRowAsHeader** en **skipLineCount** ](#scenarios-for-using-firstrowasheader-and-skiplinecount) voor voorbeeldscenario's. | True<br/>False (standaard) | Nee |
| skipLineCount | Geeft het aantal rijen dat moet worden overgeslagen bij het lezen van gegevens uit invoerbestanden. Als zowel skipLineCount als firstRowAsHeader zijn opgegeven, de regels eerst worden overgeslagen en wordt de header-informatie gelezen uit het invoerbestand. <br/><br/>Zie [scenario's voor het gebruik van firstRowAsHeader en skipLineCount](#scenarios-for-using-firstrowasheader-and-skiplinecount) voor voorbeeldscenario's. | Geheel getal | Nee | 
| treatEmptyAsNull | Hiermee bepaalt u of null of een lege tekenreeks als een null-waarde worden behandeld bij het lezen van gegevens uit een invoerbestand. | True (standaard)<br/>False | Nee |  

#### <a name="textformat-example"></a>Voorbeeld van de tekstopmaak
In het volgende voorbeeld ziet u enkele van de eigenschappen voor tekstopmaak.

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

Gebruik een escapeChar in plaats van quoteChar, de regel te vervangen door quoteChar met de volgende escapeChar:

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Scenario's voor het gebruik van firstRowAsHeader en skipLineCount

- U kopieert vanuit een bron niet bestand naar een tekstbestand en graag een kopregel die de schemametagegevens bevat (bijvoorbeeld: SQL-schema). **FirstRowAsHeader** opgeven als waar in de dataset uitvoer voor dit scenario. 
- U kopieert uit een tekstbestand met een kopregel met een niet-bestand sink en wilt neerzetten die regel. **FirstRowAsHeader** opgeven als waar in de dataset invoer.
- U wilt kopiëren uit een tekstbestand en een paar regels aan het begin van die geen gegevens- of veldnamenbestand gegevens bevatten overslaan. Geef de **skipLineCount** om aan te geven van het aantal regels dat moet worden overgeslagen. Als de rest van het bestand een kopregel bevat, kunt u ook **firstRowAsHeader**. Als zowel **skipLineCount** als **firstRowAsHeader** zijn opgegeven, de regels eerst worden overgeslagen en wordt de header-informatie gelezen uit het invoerbestand

### <a name="specifying-avroformat"></a>AvroFormat opgeven
Als de indeling is ingesteld op AvroFormat, hoeft u niet alle eigenschappen in de sectie opmaak in de sectie typeProperties opgeven. Voorbeeld:

    "format":
    {
        "type": "AvroFormat",
    }

Avro als notatie wilt gebruiken in een component-tabel, kunt u verwijzen naar [de Apache component zelfstudie](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Houd rekening met de volgende punten:  

- [Complexe gegevenstypen](http://avro.apache.org/docs/current/spec.html#schema_complex) worden niet ondersteund (records enums, matrices, kaarten, vakbonden en vaste)

### <a name="specifying-jsonformat"></a>JsonFormat opgeven

Als de indeling is ingesteld op **JsonFormat**, kunt u de volgende **optionele** eigenschappen in de sectie **Opmaak** opgeven.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| filePattern | Het patroon van de gegevens die zijn opgeslagen in elke JSON-bestand aangeven. Toegestane waarden zijn: **setOfObjects** en **arrayOfObjects**. **De standaardwaarde** is: **setOfObjects**. Zie de volgende secties voor meer informatie over deze patronen.| Nee |
| encodingName | Geef de naam van de codering. Zie voor de lijst met geldige namen van codering: de eigenschap [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) . Bijvoorbeeld: windows-1250 of shift_jis. **De standaardwaarde** is: **UTF-8**. | Nee | 
| nestingSeparator | Teken dat wordt gebruikt voor het scheiden van geneste niveaus. De standaardwaarde is '.' (punt). | Nee | 


#### <a name="setofobjects-file-pattern"></a>setOfObjects bestandspatroon

Elk bestand bevat één object of lijn-gescheiden/aaneenschakelen meerdere objecten. Wanneer deze optie is gekozen in een dataset uitvoer, produceert kopie activiteit één JSON-bestand met elk object per regel (regel gescheiden).

**één object** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**regel gescheiden JSON** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**aaneengeschakelde JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern"></a>arrayOfObjects bestandspatroon. 

Elk bestand bevat een array van objecten. 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>Voorbeeld van de JsonFormat

Als u een JSON-bestand met de volgende inhoud:  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

en u wilt kopiëren naar een Azure SQL-tabel in de volgende notatie: 

ID  | Name.First | Name.Middle | Name.Last | Tags
--- | ---------- | ----------- | --------- | ----
1 | John | Null | De Vries | ["Data Factory", 'Azure']

De dataset input type JsonFormat wordt als volgt gedefinieerd: (gedeeltelijke definitie met alleen de relevante delen)

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

Als de structuur niet is gedefinieerd, wordt de activiteit kopiëren effent de structuur standaard en kopieert u elk ding. 

#### <a name="supported-json-structure"></a>Ondersteunde JSON-structuur
Houd rekening met de volgende punten: 

- Elk object met een verzameling naam/waarde-paren wordt toegewezen aan één rij met gegevens in tabelvorm. Objecten kunnen worden genest en kunt u het afvlakken van de structuur in een dataset met het scheidingsteken dat nesten (.) standaard. Zie het [voorbeeld van de JsonFormat](#jsonformat-example) voorafgaand aan de sectie voor een voorbeeld.  
- Als de structuur is niet gedefinieerd in de dataset Data Factory, de kopie detecteert het schema van het eerste object en het hele object afvlakken. 
- Als de JSON-invoer een matrix is, zet de activiteit kopiëren de gehele matrix-waarde in een tekenreeks. U kunt met behulp van [kolommen toewijzen of filteren](#column-mapping-with-translator-rules)wilt overslaan.
- Als er dubbele namen op hetzelfde niveau, neemt de activiteit kopie laatste.
- De namen van eigenschappen zijn hoofdlettergevoelig. Twee eigenschappen met dezelfde naam maar met verschillende darmen worden beschouwd als twee afzonderlijke eigenschappen. 

### <a name="specifying-orcformat"></a>OrcFormat opgeven
Als de indeling is ingesteld op OrcFormat, hoeft u niet alle eigenschappen in de sectie opmaak in de sectie typeProperties opgeven. Voorbeeld:

    "format":
    {
        "type": "OrcFormat"
    }

> [AZURE.IMPORTANT] Als u bestanden van de ORC niet kopieert **als-is** tussen lokale en cloud opgeslagen gegevens, moet u de 8 JRE (Java Runtime Environment) op uw gateway-computer installeren. Een 64-bits-gateway vereist Java Runtime Environment, 64-bits en 32-bits gateway JRE 32-bits vereist. U kunt beide versies van [hier](http://go.microsoft.com/fwlink/?LinkId=808605)vinden. Kies het juiste bestand.

Houd rekening met de volgende punten:

-   Complexe gegevens typen niet worden ondersteund (STRUCT, MAP, lijst, Unie)
-   ORC-bestand bevat drie [Opties voor compressie](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): geen, ZLIB, SNAPPY. Data Factory ondersteunt het lezen van gegevens uit een bestand in een van deze gecomprimeerde indelingen ORC. De compressie wordt gebruikt-codec is in de metagegevens om de gegevens te lezen. Echter, bij het schrijven naar een bestand ORC, Data Factory kiest ZLIB, de standaard voor ORC. Er is momenteel geen optie voor deze instelling wijzigen. 

### <a name="specifying-parquetformat"></a>ParquetFormat opgeven
Als de indeling is ingesteld op ParquetFormat, hoeft u niet alle eigenschappen in de sectie opmaak in de sectie typeProperties opgeven. Voorbeeld:

    "format":
    {
        "type": "ParquetFormat"
    }

> [AZURE.IMPORTANT] Als u bestanden van parketvloeren niet kopieert **als-is** tussen lokale en cloud opgeslagen gegevens, moet u de 8 JRE (Java Runtime Environment) op uw gateway-computer installeren. Een 64-bits-gateway vereist Java Runtime Environment, 64-bits en 32-bits gateway JRE 32-bits vereist. U kunt beide versies van [hier](http://go.microsoft.com/fwlink/?LinkId=808605)vinden. Kies het juiste bestand.

Houd rekening met de volgende punten:

-   Complexe gegevens typen niet worden ondersteund (kaart, lijst)
-   Parketvloeren-bestand heeft de volgende compressie-opties: geen, SNAPPY, GZIP en LZO. Data Factory ondersteunt het lezen van gegevens uit een bestand in een van deze gecomprimeerde indelingen ORC. Wordt de compressiecodec in de metagegevens om de gegevens te lezen. Echter, bij het schrijven naar een bestand parketvloeren, Data Factory kiest SNAPPY, de standaard voor de indeling van parketvloeren. Er is momenteel geen optie voor deze instelling wijzigen. 
