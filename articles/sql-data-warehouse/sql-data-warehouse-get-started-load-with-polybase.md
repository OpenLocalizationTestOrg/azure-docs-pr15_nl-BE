<properties
   pageTitle="PolyBase in SQL gegevens magazijn zelfstudie | Microsoft Azure"
   description="Informatie over PolyBase en hoe u deze gebruikt voor scenario's voor gegevensopslag."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Gegevens laden met PolyBase in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)

Deze zelfstudie laat zien hoe gegevens in SQL Data Warehouse laden met behulp van AzCopy en PolyBase. Wanneer u klaar bent, weet u hoe:

- AzCopy gebruiken om gegevens te kopiëren naar Azure blobopslag
- Database-objecten als u wilt aangeven welke gegevens maken
- Een T-SQL-query om de gegevens te laden

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## <a name="prerequisites"></a>Vereisten

Stap in deze zelfstudie, wilt nodig u hebt

- SQL Data Warehouse-database.
- Een Azure opslag account van het type standaard lokaal redundante opslag (standaard-LRS) standaard Geo-redundante opslag (standaard GRS) of standaard leestoegang Geo-redundante opslag (standaard RAGRS).
- Het opdrachtregelprogramma AzCopy. Download en installeer de [nieuwste versie van AzCopy][] is geïnstalleerd met de hulpprogramma's van Microsoft Azure opslag.

    ![Extra opslag Azure](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Stap 1: Voorbeeldgegevens naar Azure blobopslag toevoegen

We moeten gegevens laden, wat voorbeeldgegevens naar een Azure blobopslag plaatsen. In deze stap, vullen we een opslag Azure blob met voorbeeldgegevens. Later, gebruiken we PolyBase deze voorbeeldgegevens in uw SQL Data Warehouse-database laden.

### <a name="a-prepare-a-sample-text-file"></a>A. Een voorbeeld van tekst-bestand voorbereiden

Een voorbeeld van tekst-bestand voorbereiden:

1. Open Kladblok en kopieer de volgende regels met gegevens in een nieuw bestand. Dit opslaan in de lokale tijdelijke map als % temp%\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B. Zoek uw blob-service-eindpunt

Zoeken in de blob-service-eindpunt:

1. Selecteer **Bladeren**in de Portal Azure > **Opslag rekeningen**.
2. Klik op de opslag-account die u wilt gebruiken.
3. Klik in de opslag account-blade BLOB 's

    ![Klik op de BLOB 's](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. De blob-service-eindpunt-URL opslaan voor later.

    ![BLOB-service-eindpunt](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Zoek uw sleutel Azure opslag

Naar de sleutel Azure opslag:

1. Selecteer **Bladeren**in de Portal Azure > **Opslag rekeningen**.
2. Klik op de opslag-account die u wilt gebruiken.
3. Selecteer **alle instellingen** > **toegangstoetsen**.
4. Klik in het vak kopiëren als u wilt een van uw access-sleutels naar het Klembord kopiëren.

    ![Sleutel Azure opslag kopiëren](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>D. Het voorbeeldbestand kopiëren naar Azure blob-opslag

De gegevens kopiëren naar Azure blob-opslag:

1. Open een opdrachtprompt en verplaats de mappen naar de installatiemap van AzCopy. Deze opdracht wordt gewijzigd in de standaardmap voor installatie op een 64-bits Windows-client.

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. Voer de volgende opdracht om het bestand te uploaden. Geef uw blob-service-eindpunt-URL voor <blob service endpoint URL> en de sleutel van de account Azure opslag voor < azure_storage_account_key >.

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Zie ook [aan de slag met het opdrachtregelprogramma AzCopy][].

### <a name="e-explore-your-blob-storage-container"></a>E. Verken uw container blob-opslag

Overzicht van het bestand u geüpload naar een blobopslag:

1. Ga terug naar de blade Blob-service.
2. Dubbelklik onder Containers, op **datacontainer**.
3. Klik op de map **datedimension** om het pad naar uw gegevens, en ziet u het geüploade bestand **DimDate2.txt**.
4. Eigenschappen, klikt u op **DimDate2.txt**.
5. Houd er rekening mee dat in de blade Blob-eigenschappen kunt u downloaden of het bestand verwijderen.

    ![Weergave opslag Azure blob](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Stap 2: Een externe tabel voor de voorbeeldgegevens maken

In deze sectie maken we een externe tabel waarin de voorbeeldgegevens.

PolyBase maakt gebruik van tabellen van externe toegang tot gegevens in Azure blob-opslag. Omdat de gegevens niet zijn opgeslagen in SQL Data Warehouse, zorgt PolyBase voor verificatie van externe gegevens met behulp van een referentie-database binnen het bereik.

Het voorbeeld in deze stap wordt deze Transact-SQL-instructies voor het maken van een externe tabel.

- Referentie binnen het bereik van [hoofdsleutel maken (Transact-SQL)][] voor het coderen van het geheim van de database.
- [Maak Database binnen het bereik van referentie (Transact-SQL)][] verificatie informatie opgeven voor uw account Azure opslag.
- [Externe gegevensbron maken (Transact-SQL)][] opgeven van de locatie van uw Azure blobopslag.
- [Maken externe bestandsindeling (Transact-SQL)][] opgeven, de indeling van uw gegevens.
- [Externe tabel maken (Transact-SQL)][] opgeven voor de definitie van de tabel en de locatie van de gegevens.

Deze query uitgevoerd tegen de SQL Data Warehouse-database. Dit maakt een externe tabel met de naam DimDate2External in het schema dbo die naar de DimDate2.txt voorbeeldgegevens in de Azure blobopslag verwijst.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


In SQL Server Explorer-Object in Visual Studio ziet u de indeling van externe gegevens uit externe bron en in de tabel DimDate2External.

![Externe tabel weergeven](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Stap 3: Gegevens laden in SQL Data Warehouse

Als de externe tabel is gemaakt, kunt u de gegevens geladen in een nieuwe tabel of invoegen in een bestaande tabel.

- De instructie [CREATE tabel AS SELECT (Transact-SQL)][] voor het laden van de gegevens in een nieuwe tabel, worden uitgevoerd. De nieuwe tabel worden de kolommen in de query met de naam hebben. De gegevenstypen van de kolommen overeen met de gegevenstypen in de definitie van de externe tabel.
- Gebruik het om gegevens te laden in een bestaande tabel, de [invoegen... Selecteer (Transact-SQL)][] instructie.

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Stap 4: Statistieken maken voor uw nieuwe geladen gegevens

SQL Data Warehouse niet automatisch maken of statistieken automatisch bijwerken. Voor hoge prestaties, is het daarom belangrijk om statistieken te maken voor elke kolom van elke tabel na de eerste keer worden geladen. Het is ook belangrijk dat statistieken na ingrijpende wijzigingen in de gegevens bijwerken.

In dit voorbeeld wordt één kolom statistieken op de nieuwe DimDate2-tabel gemaakt.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

Zie voor meer informatie, [Statistieken][].  


## <a name="next-steps"></a>Volgende stappen
Zie de [handleiding PolyBase][] voor verdere informatie die u weten moet als u een oplossing ontwikkelen waarmee PolyBase worden gebruikt.

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[Handleiding voor PolyBase]: ./sql-data-warehouse-load-polybase-guide.md
[Aan de slag met het opdrachtregelprogramma AzCopy]: ../storage/storage-use-azcopy.md
[meest recente versie van AzCopy]: ../storage/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[MAKEN van gegevens uit externe bron (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[MAKEN van externe BESTANDSINDELING (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[EXTERNE tabel (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[Maak de tabel AS SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[INVOEGEN... Selecteer Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[Maak de hoofdsleutel (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[MAKEN van de DATABASE binnen bereik van referentie (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
