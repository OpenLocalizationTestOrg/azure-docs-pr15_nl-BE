## <a name="fileshare-dataset-type-properties"></a>Eigenschappen van dataset bestandsshare

Zie het artikel [gegevenssets maken](../articles/data-factory/data-factory-create-datasets.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van datasets. Secties zoals structuur, beschikbaarheid en het beleid van een dataset JSON zijn vergelijkbaar voor alle typen van de dataset. 

De sectie **typeProperties** is verschillend voor elk type dataset. Het biedt informatie die specifiek is voor het type dataset. De sectie typeProperties voor een dataset van het type **bestandsshare** dataset heeft de volgende eigenschappen:

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
folderPath | Subpad naar de map. Gebruik de escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppeld service en dataset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap in combinatie met **partitionBy** mappaden op basis van een segment zijn begin/einde-datums en tijden. | Ja
Bestandsnaam | Geef de naam van het bestand in het **mappad** als u wilt dat de tabel om te verwijzen naar een bepaald bestand in de map. Als u een waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Als de bestandsnaam is niet opgegeven voor een dataset uitvoer, de naam van het gegenereerde bestand in de volgende zou zijn deze indeling: <br/><br/>Gegevens. <Guid>.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Nee
partitionedBy | partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, bestandsnaam voor de reeks tijdgegevens. Bijvoorbeeld folderPath parameters voor elk uur van de gegevens. | Nee
Indeling | De volgende notatietypen worden ondersteund: **tekstopmaak**, **AvroFormat**, **JsonFormat**en **OrcFormat**. Stel de eigenschap **type** onder notatie op een van deze waarden. Zie [Tekstopmaak opgeven](#specifying-textformat), [AvroFormat op te geven](#specifying-avroformat), [Geven JsonFormat](#specifying-jsonformat)en [OrcFormat geven](#specifying-orcformat) secties voor meer informatie. Als u bestanden wilt kopiëren als-is tussen bestand gebaseerde winkels (binaire kopie), kunt u de opmaaksectie in beide definities van de invoer en uitvoer dataset overslaan. | Nee
fileFilter | Een filter moet worden gebruikt voor het selecteren van een subset van bestanden in het mappad in plaats van alle bestanden opgeven.<br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (enkel teken).<br/><br/>Voorbeeld 1:`"fileFilter": "*.log"`<br/>Voorbeeld 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter is van toepassing voor een invoer bestandsshare dataset. Deze eigenschap wordt niet ondersteund met HDFS.  | Nee
| compressie | Geef het type en de mate van compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, en **BZip2** en ondersteunde niveaus zijn: **optimale** en **snelst**. Compressie-instellingen worden momenteel niet ondersteund voor gegevens in **AvroFormat** of **OrcFormat**. Voor meer informatie, Zie de sectie [compressie-ondersteuning](#compression-support) .  | Nee |
| useBinaryTransfer | Opgeven of binaire overdrachtsmodus te gebruiken. True als binaire modus en false ASCII. Standaard waarde: True. Deze eigenschap kan alleen worden gebruikt wanneer de bijbehorende gekoppelde service-type is van het type: FtpServer. | Nee | 
 

> [AZURE.NOTE] bestandsnaam en fileFilter worden niet tegelijkertijd gebruikt.

### <a name="using-partionedby-property"></a>PartionedBy, eigenschap

Zoals vermeld in het vorige gedeelte, kunt u een dynamische folderPath, bestandsnaam voor de reeks tijdgegevens met partitionedBy. U kunt dit doen met de Data Factory-macro's en de systeemvariabele SliceStart, SliceEnd, die de logische periode voor een bepaald segment aangeven. 

Zie informatie over time series datasets, plannen en segmenten, [Gegevenssets maken](../articles/data-factory/data-factory-create-datasets.md), [planning en uitvoering](../articles/data-factory/data-factory-scheduling-and-execution.md)en [Pijpleidingen maken van](../articles/data-factory/data-factory-create-pipelines.md) artikelen. 

#### <a name="sample-1"></a>Voorbeeld 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

In dit voorbeeld {segment} wordt vervangen door de waarde van Data Factory systeemvariabele SliceStart in de notatie (YYYYMMDDHH). De SliceStart verwijst naar de begintijd van het segment. Het mappad verschilt voor elk segment. Voorbeeld: wikisampledataout-wikidatagateway/2014100103 of wikisampledataout-wikidatagateway/2014100104.

#### <a name="sample-2"></a>Voorbeeld 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

Jaar, maand, dag en tijd van de SliceStart in dit voorbeeld worden uitgepakt naar afzonderlijke variabelen die worden gebruikt door de eigenschappen van het mappad en de bestandsnaam.
