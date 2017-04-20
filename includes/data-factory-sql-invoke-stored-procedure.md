## <a name="invoking-stored-procedure-for-sql-sink"></a>Het aanroepen van een opgeslagen procedure voor het opvangen van SQL

Bij het kopiëren van gegevens in SQL Server of Azure SQL/SQL Server-Database, wordt een gebruiker opgegeven opgeslagen procedure kan worden geconfigureerd en geactiveerd met aanvullende parameters. 

Een opgeslagen procedure kan worden gebruikt bij het kopiëren van ingebouwde mechanismen doen niet het doel dienen. Dit wordt meestal gebruikt bij de verwerking van extra (samenvoegen kolommen invoegen in meerdere tabellen... extra waarden opzoeken) moet worden uitgevoerd voordat het uiteindelijke invoegen van gegevens in de doeltabel. 

U kunt een opgeslagen procedure van keuze kan aanroepen. In het volgende voorbeeld ziet u hoe u een opgeslagen procedure gebruiken om te doen een eenvoudige invoegen in een tabel in de database. 

**Dataset-uitvoer**

In dit voorbeeld type is ingesteld op: SqlServerTable. Stel deze in op AzureSqlTable om te gebruiken met een Azure SQL-database. 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
De sectie SqlSink in kopie activiteit JSON als volgt definiëren. Aanroepen van een opgeslagen procedure tijdens het invoegen, de eigenschappen SqlWriterStoredProcedureName en SqlWriterTableType beide zijn nodig.

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

In de database, definieert u de opgeslagen procedure met dezelfde naam als SqlWriterStoredProcedureName. Ingevoerde gegevens uit de opgegeven bron- en invoegen verwerkt in de uitvoertabel. U ziet de parameternaam van de van de opgeslagen procedure moet hetzelfde zijn als de tabelnaam in de tabel JSON-bestand gedefinieerd zijn.

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

In de database, definieert u het tabeltype met dezelfde naam als SqlWriterTableType. U ziet het schema van het tabeltype moeten hetzelfde zijn als het schema die wordt geretourneerd door de ingevoerde gegevens.

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

De functie opgeslagen procedure maakt gebruik van de [Parameters van de Table-Valued](https://msdn.microsoft.com/library/bb675163.aspx).