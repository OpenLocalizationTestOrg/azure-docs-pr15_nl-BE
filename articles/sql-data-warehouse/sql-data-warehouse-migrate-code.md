<properties
   pageTitle="De SQL-code migreren naar SQL Data Warehouse | Microsoft Azure"
   description="Tips voor het migreren van uw SQL-code naar Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/02/2016"
   ms.author="lodipalm;barbkess;sonyama;jrj"/>

# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>De SQL-code migreren naar SQL Data Warehouse

Wanneer uw code vanuit een andere database migreren naar het datawarehouse SQL, moet u waarschijnlijk wijzigingen aanbrengen in uw code base. Sommige functies van SQL Data Warehouse kunnen de prestaties aanzienlijk verbeteren zoals ze zijn bedoeld om te werken in een gedistribueerde wijze. Echter wilt behouden en prestaties van de schaal, zijn voor sommige functies ook niet beschikbaar.

## <a name="common-t-sql-limitations"></a>Algemene beperkingen van T-SQL

De volgende lijst bevat een overzicht van de meest gebruikte functie die niet worden ondersteund in Azure SQL Data Warehouse. De koppelingen verwijzen naar oplossingen voor de niet-ondersteunde functie:

- [ANSI-joins updates][]
- [ANSI-joins op verwijderen][]
- [overzicht van samenvoegen][]
- joins, cross-database
- [cursors][]
- [SELECTEER... IN][]
- [INVOEGEN... EXEC][]
- OUTPUT-component
- in line door de gebruiker gedefinieerde functies
- functies met meerdere instructies
- [algemene tabelexpressies](#Common-table-expressions)
- [recursieve algemene tabelexpressies (CTE)] (#Recursive-common-table-expressions-(CTE)
- CLR-functies en procedures
- $partition, functie
- tabelvariabelen
- de parameters voor waarde
- gedistribueerde transacties
- Commit / rollback werk
- transactie opslaan
- contexten worden uitgevoerd (EXECUTE AS)
- [Group by, component met updatepakket / kubus / stelt opties voor groeperen][]
- [geneste niveaus dan 8][]
- [werken met weergaven][]
- [gebruik van selecteren voor de toewijzing van variabele][]
- [geen MAX gegevenstype voor dynamische SQL-reeksen][]

Gelukkig kunnen de meeste van deze beperkingen worden gewerkt rond. Uitleg wordt gegeven in de ontwikkeling van relevante artikelen waarnaar hierboven wordt verwezen.

## <a name="supported-cte-features"></a>Ondersteunde CTE-functies

Algemene tabelexpressies (CTE's) worden gedeeltelijk ondersteund in SQL Data Warehouse.  De volgende CTE-functies worden momenteel ondersteund:

- Een CTE kan worden opgegeven in een SELECT-instructie.
- Een CTE kan worden opgegeven in de instructie CREATE VIEW.
- Een CTE kan worden opgegeven in een instructie maken tabel als selecteren (CTAS).
- Een CTE kan worden opgegeven in een instructie maken externe tabel als selecteren (CRTAS).
- Een CTE kan worden opgegeven in een instructie maken externe tabel als selecteren (CETAS).
- Een externe tabel kan worden verwezen vanuit een CTE.
- Een externe tabel kan worden verwezen vanuit een CTE.
- Meerdere definities van de CTE query kunnen worden gedefinieerd in een CTE.

## <a name="cte-limitations"></a>CTE beperkingen

Algemene tabelexpressies hebben enkele beperkingen in SQL Data Warehouse met inbegrip van:

- Een CTE moet worden gevolgd door één SELECT-instructie. INSERT, UPDATE, DELETE en samenvoegen-instructies worden niet ondersteund.
- Een algemene tabelexpressie die verwijzingen naar zichzelf (een recursieve algemene tabelexpressie bevat) wordt niet ondersteund (Zie onder sectie).
- Meer dan één met de component op te geven in een CTE is niet toegestaan. Bijvoorbeeld, als een CTE_query_definition een subquery bevat, mag die subquery niet een geneste met de component die een andere CTE definieert.
- Een ORDER BY-component kan niet worden gebruikt in de CTE_query_definition, behalve wanneer een TOP-component is opgegeven.
- Wanneer een CTE in een instructie die deel uitmaakt van een batch wordt gebruikt, moet de instructie voordat deze worden gevolgd door een puntkomma.
- Als in jaarrekeningen die worden opgesteld door de sp_prepare gebruikt, werken de CTE's wordt net als andere SELECT-instructies in PDW. Echter, als CTE's worden gebruikt als onderdeel van CETAS voorbereid door sp_prepare, het gedrag kan uitstellen van SQL Server en andere instructies PDW vanwege de manier waarop binding is geïmplementeerd voor sp_prepare. Als bepaalde verwijzingen naar de die CTE met behulp van een verkeerde kolom die niet in de CTE bestaat, de sp_prepare worden doorgegeven zonder de fout te detecteren, maar de fout zal worden gegenereerd tijdens de sp_execute in plaats daarvan.

## <a name="recursive-ctes"></a>Recursieve CTE 's

Recursieve CTE's worden niet ondersteund in SQL Data Warehouse.  De migraion van recursieve CTE kan enigszins voltooid zijn en het beste is om op te splitsen het in meerdere stappen. Meestal kunt u een lus gebruiken en een tijdelijke tabel te vullen als u de tussentijdse recursieve query's doorlopen. Als u de tijdelijke tabel wordt ingevuld kunt u de gegevens als een enkele resultaatset terugkeren. Een soortgelijke benadering is gebruikt voor het oplossen van `GROUP BY WITH CUBE` in het artikel [group by, component met updatepakket / kubus / stelt opties voor groeperen][] .

## <a name="unsupported-system-functions"></a>Voor niet-ondersteunde systeemfuncties

Er zijn ook sommige systeemfuncties die niet worden ondersteund. Enkele van de belangrijkste zijn wellicht hebt u meestal gebruikt in data warehousing zijn:

- NEWSEQUENTIALID()
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE()

Sommige van deze problemen kan worden gewerkt rond.

## <a name="rowcount-workaround"></a>@@ROWCOUNTtijdelijke oplossing

Gebrek aan ondersteuning voor het omzeilen van @@ROWCOUNT, maken van een opgeslagen procedure die wordt uitgevoerd en het laatste aantal rijen ophalen uit sys.dm_pdw_request_steps `EXEC LastRowCount` na een DML-instructie.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een volledige lijst van alle ondersteunde T-SQL-instructies, [Transact-SQL-onderwerpen][].

<!--Image references-->

<!--Article references-->
[ANSI-joins updates]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI-joins op verwijderen]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[overzicht van samenvoegen]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[INVOEGEN... EXEC]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Onderwerpen van Transact-SQL]: ./sql-data-warehouse-reference-tsql-statements.md

[cursors]: ./sql-data-warehouse-develop-loops.md
[SELECTEER... IN]: ./sql-data-warehouse-develop-ctas.md#selectinto
[Group by, component met updatepakket / kubus / stelt opties voor groeperen]: ./sql-data-warehouse-develop-group-by-options.md
[geneste niveaus dan 8]: ./sql-data-warehouse-develop-transactions.md
[werken met weergaven]: ./sql-data-warehouse-develop-views.md
[gebruik van selecteren voor de toewijzing van variabele]: ./sql-data-warehouse-develop-variable-assignment.md
[geen MAX gegevenstype voor dynamische SQL-reeksen]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
