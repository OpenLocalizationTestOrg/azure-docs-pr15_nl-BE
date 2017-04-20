## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Structuurdefinitie voor rechthoekige datasets opgeven
Het gedeelte van de structuur in de datasets JSON is een **optionele** sectie voor rechthoekige tabellen (met rijen en kolommen) en bevat een collectie van kolommen van de tabel. De sectie structuur gebruikt u voor beide bieden informatie over conversie- of doet kolomtoewijzingen. De volgende secties komen deze functies gedetailleerd. 

Elke kolom bevat de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| naam | De naam van de kolom. | Ja |
| type | Het gegevenstype van de kolom. Zie type conversies sectie hieronder voor meer details over wanneer moet u type-informatie | Nee |
| cultuur | Cultuur moet worden gebruikt wanneer het type is opgegeven en .NET type datumtijd of Datetimeoffset op basis van .NET. De standaardwaarde is ' en-us '.  | Nee |
| indeling | Notatiereeks die moet worden gebruikt wanneer het type is opgegeven en .NET Typ datumtijd of Datetimeoffset. | Nee |

In het volgende voorbeeld ziet u de sectie structuur JSON voor een tabel met drie kolommen gebruikers-id, naam en lastlogindate.

    "structure": 
    [
        { "name": "userid"},
        { "name": "name"},
        { "name": "lastlogindate"}
    ],

Gebruik de volgende richtlijnen voor 'structuur' informatie wordt opgenomen en wat u wilt opnemen in de sectie **structuur** .

- Doen kolomtoewijzing van specifieke kolommen aan specifieke kolommen in de sink **voor gestructureerde gegevensbronnen** die het schema en het type gegevens en de gegevens zelf (bronnen zoals SQL Server, Oracle, Azure tabel enz.), moet u de sectie 'structuur' alleen als u wilt opslaan en hun namen niet hetzelfde zijn (Zie kolom toewijzing sectie hieronder voor meer informatie). 

    Zoals hierboven vermeld, is de type-informatie is optioneel in de sectie 'structuur'. Voor gestructureerde bronnen, informatie over het type nog beschikbaar als onderdeel van de definitie van de gegevensset in het gegevensarchief, dus u moet niet type informatie opnemen wanneer u de sectie 'structuur' opneemt.
- **Voor meer gegevensbronnen (specifiek Azure blob)-schema** die kunt u gegevens op te slaan zonder een schema of type informatie met de gegevens op te slaan. Voor deze typen gegevensbronnen moet u 'structuur' opnemen in de volgende gevallen 2:
    - U wilt doen kolomtoewijzing.
    - Wanneer de dataset een bron van de activiteit van een kopie is, kunt u informatie over het type in 'structuur' bieden en data factory dit type informatie voor conversie naar de native-typen voor de sink wordt gebruikt. Zie artikel [verplaatsen van gegevens van en naar Azure Blob](../articles/data-factory/data-factory-azure-blob-connector.md) voor meer informatie.

### <a name="supported-net-based-types"></a>Ondersteund. Typen op basis van het NET 
Data factory ondersteunt de volgende CLS compatibel is met .NET op basis van typewaarden voor het verstrekken van informatie over het type in "structuur" voor meer gegevensbronnen zoals Azure blob-schema.

- Int16
- Int32 
- Int64
- Enkele
- Getal met dubbele precisie
- Decimaal
- Byte]
- BOOL
- Tekenreeks 
- GUID
- Datum/tijd
- DateTimeOffset
- TimeSpan 

Voor datum/tijd- en Datetimeoffset kunt u desgewenst ook "cultuur" en "format" tekenreeks ter vergemakkelijking van het parseren van het aangepaste datum/tijd-tekenreeks opgeven. Zie voorbeeld voor typeconversie hieronder.

