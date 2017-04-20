<properties
   pageTitle="Lussen in SQL datawarehouse | Microsoft Azure"
   description="Tips voor Transact-SQL-lussen en vervangt cursors in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="loops-in-sql-data-warehouse"></a>Lussen in SQL datawarehouse
De lus [terwijl][] ondersteunt SQL Data Warehouse voor het herhaaldelijk uitvoeren van instructie blokken. Deze blijven voor als de opgegeven voorwaarden waar zijn, of totdat de code specifiek wordt beëindigd de lus met behulp van de `BREAK` trefwoord. Lussen zijn vooral nuttig voor het vervangen van cursors die zijn gedefinieerd in de SQL-code. Gelukkig alleen verschillende bijna alle cursors die zijn geschreven in de SQL-code van de snel vooruit worden gelezen. [Terwijl] de lussen zijn dus een uitstekend alternatief als u dat u hoeft merkt te vervangen.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Gebruik te maken van lussen en vervangen van cursors in SQL Data Warehouse
Echter, vooraleer in hoofd eerst vraagt u uzelf de volgende vraag: "Kan deze cursor opnieuw naar worden geschreven instellen op basis van bewerkingen gebruiken?". In veel gevallen is het antwoord Ja is en is vaak de beste aanpak. Een set op basis van de bewerking vaak aanzienlijk sneller dan een iteratieve, per rij aanpak wordt uitgevoerd.

Vooruitspoelen cursors voor alleen-lezen kunnen eenvoudig worden vervangen door een herhaling constructie. Hieronder vindt u een eenvoudig voorbeeld. Dit codevoorbeeld worden de statistieken voor elke tabel in de database bijgewerkt. Door iteratie van de tabellen in de lus kunnen we voor het uitvoeren van elke opdracht in de reeks.

Maak eerst een tijdelijke tabel met een unieke rijnummer waarmee de afzonderlijke overzichten:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Ten tweede initialiseren van de variabelen die nodig zijn voor het uitvoeren van de lus:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Nu op een uitvoeren-instructies in een lus tegelijk:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Ten slotte zet de tijdelijke tabel gemaakt in de eerste stap

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[TIJDENS]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
