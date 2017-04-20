<properties
   pageTitle="Toewijzen van variabelen in SQL Data Warehouse | Microsoft Azure"
   description="Tips voor het toewijzen van variabelen in Azure SQL Data Warehouse Transact-SQL voor het ontwikkelen van oplossingen."
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

# <a name="assign-variables-in-sql-data-warehouse"></a>Toewijzen van variabelen in SQL Data Warehouse
Variabelen in SQL Data Warehouse worden ingesteld met de `DECLARE` instructie of de `SET` instructie.

De volgende zijn perfect geldig manieren om de waarde van een variabele instellen:

## <a name="setting-variables-with-declare"></a>Instelling van variabelen met DECLARE

Het initialiseren van variabelen met DECLARE is een van de meest flexibele manieren om de waarde van een variabele instellen in SQL Data Warehouse.

```sql
DECLARE @v  int = 0
;
```

U kunt ook DECLARE gebruiken meer dan één variabele instellen op een tijdstip. U kunt niet gebruiken `SELECT` of `UPDATE` om dit te doen:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

U kan niet geïnitialiseerd en gebruikt een variabele in de instructie DECLARE. Ter illustratie van het punt in het onderstaande voorbeeld is **niet** toegestaan als @p1 is geïnitialiseerd en gebruikt in de instructie DECLARE. Dit zal resulteren in een fout.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Met de SET waarden in te stellen
Een veelgebruikte methode voor het instellen van een enkele variabele is.

De onderstaande voorbeelden zijn geldige manieren van het instellen van een variabele is ingesteld:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

U kunt alleen één variabele tegelijk instellen met ingesteld. Zoals u hierboven kunt zien zijn samengestelde operators toegestane.

## <a name="limitations"></a>Beperkingen
Kunt u selecteren of de UPDATE voor toewijzing van variabele.


## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
