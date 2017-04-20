<properties
   pageTitle="Gegevenstypen voor tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Aan de slag met Azure SQL Data Warehouse tabellen-gegevenstypen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="data-types-for-tables-in-sql-data-warehouse"></a>Gegevenstypen voor tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partitie][]
- [Statistieken][]
- [Tijdelijke][]

Datawarehouse SQL ondersteunt gebruikte de meest gegevenstypen.  Hieronder vindt u een overzicht van de gegevenstypen die worden ondersteund door SQL Data Warehouse.  Ondersteuning voor meer informatie over het gegevenstype, Zie [tabel maken][].

|**Ondersteunde gegevenstypen**|||
|---|---|---|
[bigint][]|[decimaal][]|[smallint][]|
[binair][]|[Float][]|[smallmoney][]|
[bits][]|[int][]|[Sysname][]|
[CHAR][]|[Money][]|[tijd][]|
[datum][]|[nchar][]|[tinyint][]|
[datum/tijd][]|[nvarchar][]|[unieke id][]|
[datetime2][]|[real][]|[varbinary][]|
[DateTimeOffset][]|[smalldatetime][]|[varchar][]|


## <a name="data-type-best-practices"></a>Aanbevolen procedures voor gegevenstype

 Bij het definiëren van de kolomtypen verbeteren met behulp van het kleinst mogelijke gegevenstype die ondersteuning voor uw gegevens bieden prestaties van query's. Dit is vooral van belang voor de teken- en VARCHAR kolommen. Als de langste waarde in een kolom bestaat uit 25 tekens, moet u vervolgens de kolom definiëren als VARCHAR(25). Niet alle tekenkolommen definiëren op een grote standaardlengte. Bovendien kolommen definiëren als VARCHAR, wanneer dat is alles dat nodig is dan [NVARCHAR][]gebruiken.  NVARCHAR(4000) of VARCHAR(8000), indien mogelijk in plaats van NVARCHAR(MAX) of VARCHAR(MAX) gebruiken.

## <a name="polybase-limitation"></a>Beperking van Polybase

Als u de tabellen geladen via Polybase, uw tabellen definiëren zodat de maximale mogelijke rijgrootte, met inbegrip van de volledige lengte van kolommen met variabele lengte niet meer dan 32.767 bytes.  Terwijl u een rij met gegevens met variabele lengte die kan groter zijn dan deze breedte en rijen met BCP laden definiëren kunt, kunt u worden niet Polybase gebruiken om deze gegevens te laden.  Polybase ondersteuning voor brede rijen worden binnenkort toegevoegd.

## <a name="unsupported-data-types"></a>Niet-ondersteunde gegevenstypen

Als u de database van een ander SQL platform zoals Azure SQL-Database, migreert als u migreert, kunnen bepaalde gegevenstypen die niet worden ondersteund in SQL Data Warehouse optreden.  Hieronder vindt u niet-ondersteunde gegevenstypen, alsmede een aantal alternatieven die u in plaats van een niet-ondersteunde gegevenstypen gebruiken kunt.

|Gegevenstype|Tijdelijke oplossing|
|---|---|
|[geometrie][]|[varbinary][]|
|[Geografie][]|[varbinary][]|
|[activiteitknooppunt][]|[nvarchar][] (4000)|
|[afbeelding][ntext,text,image]|[varbinary][]|
|[tekst][ntext,text,image]|[varchar][]|
|[ntext][ntext,text,image]|[nvarchar][]|
|[sql_variant][]|Kolom splitsen in meerdere kolommen van sterk getypeerde.|
|[tabel][]|Converteren naar tijdelijke tabellen.|
|[tijdstempel][]|Bijwerken van de code voor het gebruik van [datetime2][] en `CURRENT_TIMESTAMP` functie.  Alleen de constanten worden ondersteund als standaard, dus current_timestamp kan niet worden gedefinieerd als een standaardbeperking. Als u rijwaarden versie migreren van een getypte tijdstempelkolom moet vervolgens gebruiken [binaire][](8) of [VARBINARY][binaire](8) voor niet NULL of NULL-Versiewaarden rij.|
|[XML][]|[varchar][]|
|[door gebruiker gedefinieerde typen][]|converteren naar de native-typen waar mogelijk|
|standaardwaarden|letterlijke waarden en constanten alleen ondersteuning voor standaardwaarden.  Niet-deterministische expressies en functies, zoals `GETDATE()` of `CURRENT_TIMESTAMP`, worden niet ondersteund.|

De onderstaande SQL kan worden uitgevoerd op uw huidige SQL-database voor kolommen die niet wordt ondersteund door Azure SQL Data Warehouse:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie, Zie de artikelen op [Tabel overzicht][Overzicht], [distributie van een tabel][verdelen], [indexeren van een tabel],[Index], [partitioneren van een tabel][partitie], [Onderhouden van tabelstatistieken],[Statistieken] en[tijdelijke] [Tijdelijke tabellen].  Voor meer informatie over aanbevolen procedures Zie [Aanbevolen procedures voor magazijn SQL-gegevens][].

<!--Image references-->

<!--Article references-->
[Overzicht]: ./sql-data-warehouse-tables-overview.md
[Gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[Distribueren]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partitie]: ./sql-data-warehouse-tables-partition.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[Tijdelijke]: ./sql-data-warehouse-tables-temporary.md
[Aanbevolen procedures voor magazijn SQL-gegevens]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[tabel maken]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[binair]: https://msdn.microsoft.com/library/ms188362.aspx
[bits]: https://msdn.microsoft.com/library/ms177603.aspx
[CHAR]: https://msdn.microsoft.com/library/ms176089.aspx
[datum]: https://msdn.microsoft.com/library/bb630352.aspx
[datum/tijd]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[DateTimeOffset]: https://msdn.microsoft.com/library/bb630289.aspx
[decimaal]: https://msdn.microsoft.com/library/ms187746.aspx
[Float]: https://msdn.microsoft.com/library/ms173773.aspx
[geometrie]: https://msdn.microsoft.com/library/cc280487.aspx
[Geografie]: https://msdn.microsoft.com/library/cc280766.aspx
[activiteitknooppunt]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[Money]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[real]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[Sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[tabel]: https://msdn.microsoft.com/library/ms175010.aspx
[tijd]: https://msdn.microsoft.com/library/bb677243.aspx
[tijdstempel]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[unieke id]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[XML]: https://msdn.microsoft.com/library/ms187339.aspx
[door gebruiker gedefinieerde typen]: https://msdn.microsoft.com/library/ms131694.aspx
