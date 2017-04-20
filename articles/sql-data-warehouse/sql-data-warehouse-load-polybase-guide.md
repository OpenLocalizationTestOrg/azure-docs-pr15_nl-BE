<properties
   pageTitle="Handleiding voor het gebruik van PolyBase in SQL Data Warehouse | Microsoft Azure"
   description="Richtsnoeren en aanbevelingen voor het gebruik van PolyBase in SQL Data Warehouse-scenario's."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Handleiding voor het gebruik van PolyBase in SQL Data Warehouse

Deze handleiding biedt praktische informatie voor het gebruik van PolyBase in SQL Data Warehouse.

Zie de [gegevens laden met PolyBase][] zelfstudie aan de slag.


## <a name="rotating-storage-keys"></a>Roterende sleutels opslag

Van tijd tot tijd zult u de toegangstoets wijzigen in de blob-opslag om veiligheidsredenen.

De meest elegante manier voor het uitvoeren van deze taak is te volgen van een proces dat bekend staat als 'draaien de sleutels'. U hebt mogelijk gezien dat er twee sleutels van opslag voor uw account blob-opslag. Dit is zodat u overstappen kunt.

Uw account Azure opslag sleutels draaien is een eenvoudige drie stappen

1. Tweede database binnen het bereik van referenties op basis van de toegangstoets secundaire opslag maken
2. Tweede externe gegevensbron op basis van deze nieuwe referentie maken
3. DROP en de externe tabellen verwijzen naar de externe gegevensbron

Wanneer u hebt gemigreerd alle externe tabellen met de externe gegevensbron en vervolgens kunt u de clean-up taken uitvoeren:

1. Eerste externe gegevensbron verwijderen
2. De eerste database neerzetten binnen het bereik van referenties op basis van de primaire opslag toegangstoets
3. Log in op Azure en de sleutel primaire access gereed voor de volgende keer opnieuw genereren

## <a name="query-azure-blob-storage-data"></a>Querygegevens Azure blob-opslag
De naam van de query's voor externe tabellen gewoon gebruiken alsof het een relationele tabel is.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] Een query op een externe tabel kan mislukken met de fout *'Query afgebroken--de maximale drempelwaarde weigeren werd bereikt tijdens het lezen van een externe bron'*. Dit geeft aan dat de externe gegevens *Ongeldige* records bevat. Een record wordt beschouwd als 'vuil' als u de werkelijke gegevens typen of aantal kolommen komen niet overeen met de kolomdefinities van de externe tabel of als de gegevens niet aan de opgegeven externe bestandsindeling voldoen. U corrigeert dit door ervoor te zorgen dat de externe tabel en het externe bestandsindelingsdefinities juist zijn en dat de externe gegevens aan deze definities voldoet. Voor het geval een subset van records in externe gegevens zijn gewijzigd, kunt u deze records voor de query's weigeren via de opties afwijzen in DDL externe tabel maken.


## <a name="load-data-from-azure-blob-storage"></a>Gegevens laden van Azure blob-opslag
In dit voorbeeld laadt gegevens van Azure blob-opslag met SQL Data Warehouse-database.

Het rechtstreeks opslaan van gegevens, verwijdert u de overdrachtstijd van gegevens voor query's. Opslag van gegevens met een index columnstore verbetert de prestaties van query's voor analyse-query's door tot 10 x.

In dit voorbeeld gebruikt de instructie CREATE tabel AS SELECT om gegevens te laden. De nieuwe tabel overgenomen de kolommen met de naam in de query. Uit de definitie van de externe tabel worden de gegevenstypen van de kolommen.

CREATE tabel AS SELECT is een uiterst zodat Transact-SQL-instructie die de gegevens ter aanvulling van de compute nodes van uw SQL Data Warehouse worden geladen.  Het is oorspronkelijk ontwikkeld voor de massively parallel processing (MPP) motor in Analytics Platform systeem en is nu in SQL Data Warehouse.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Zie [CREATE tabel AS SELECT (Transact-SQL)][].

## <a name="create-statistics-on-newly-loaded-data"></a>Statistieken op het nieuw geladen gegevens maken

Azure SQL Data Warehouse biedt nog geen ondersteuning voor automatische maken of automatische update statistieken.  Om u de beste prestaties van uw query's, is het belangrijk dat statistieken op alle kolommen van alle tabellen worden gemaakt na de eerste keer worden geladen of belangrijke wijzigingen optreden in de gegevens.  Zie het onderwerp van de [Statistieken][] in de groep onderwerpen opstellen voor een gedetailleerde uitleg van de statistieken.  Hieronder is een snelle voorbeeld van het maken van statistieken over de tabellen bevatten geladen in het volgende voorbeeld.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Gegevens exporteren naar Azure blob-opslag
In dit gedeelte ziet u hoe gegevens te exporteren uit het datawarehouse SQL Azure blob-opslag. In dit voorbeeld wordt de maken externe tabel AS SELECT is een uiterst zodat Transact-SQL-instructie in parallelle gegevens exporteren vanuit de compute nodes.

Het volgende voorbeeld wordt een tabel in een externe Weblogs2014 met kolomdefinities en gegevens uit dbo. Weblogs-tabel. Definitie van de externe tabel wordt opgeslagen in een datawarehouse SQL en de resultaten van de SELECT-instructie worden geëxporteerd naar de map "/ archiveren/log2014 /" onder de blob-container door de gegevensbron wordt opgegeven. De gegevens worden geëxporteerd in de indeling van de opgegeven tekst.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Door de vereiste PolyBase UTF-8
Ondersteunt de gegevensbestanden die UTF-8 gecodeerd zijn geladen op PolyBase aanwezig. Als UTF-8 gebruikt dezelfde tekencodering als ASCII-PolyBase wordt ook uitgevoerd ondersteuning laden gegevens ASCII is gecodeerd. Echter PolyBase biedt geen ondersteuning voor codering van andere tekens zoals in UTF-16 / Unicode of uitgebreide ASCII-tekens. Extended ASCII bevat tekens met accenten, zoals de umlaut die veel gebruikt in het Duits wordt.

U kunt oplossen door deze eis is het beste antwoord opnieuw schrijven naar UTF-8-codering.

Er zijn verschillende manieren om dit te doen. Hieronder worden twee benaderingen met Powershell:

### <a name="simple-example-for-small-files"></a>Eenvoudig voorbeeld voor kleine bestanden

Hieronder vindt u een eenvoudige één regel Powershell script waarmee het bestand wordt gemaakt.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

Terwijl dit een eenvoudige manier om de gegevens opnieuw coderen is is het echter in geen geval het meest efficiënt zijn. Het i/o streaming voorbeeld hieronder is veel, veel sneller en hetzelfde resultaat bereikt.

### <a name="io-streaming-example-for-larger-files"></a>Voorbeeld van de i/o-Streaming voor grotere bestanden

Het volgende codevoorbeeld is complexer, maar tijdens de stroomsgewijze verzending rijen met gegevens van de bron naar het doel is veel efficiënter. Gebruik deze methode voor grotere bestanden.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het verplaatsen van gegevens naar SQL Data Warehouse, [gegevens migreren-overzicht][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Gegevens laden met PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[gegevens migreren-overzicht]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[Maken externe BESTANDSINDELING (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026) .aspx [maken externe tabel (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspxx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[Maak de tabel AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
