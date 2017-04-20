<properties
   pageTitle="Opgeslagen procedures in SQL Data Warehouse | Microsoft Azure"
   description="Tips voor de uitvoering van opgeslagen procedures in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="stored-procedures-in-sql-data-warehouse"></a>Opgeslagen procedures in SQL Data Warehouse

SQL Data Warehouse ondersteunt veel functies gevonden in SQL Server Transact-SQL. Er zijn nog belangrijker is geschaald uitbreiden-specifieke functies die we benutten willen om de prestaties van uw oplossing.

Als u wilt behouden zijn de omvang en de prestaties van SQL Data Warehouse echter ook bepaalde functies en functionaliteit die u hebt doorgevoerd verschillen en anderen die niet worden ondersteund.

In dit artikel wordt uitgelegd hoe u opgeslagen procedures in SQL Data Warehouse te implementeren.

## <a name="introducing-stored-procedures"></a>Introductie van opgeslagen procedures
Opgeslagen procedures vormen een uitstekende manier voor het inkapselen van de SQL-code; het opslaan van het dicht bij de gegevens in het gegevenswarehouse. Door de code in eenheden die waarmee opgeslagen procedures ontwikkelaars hun oplossingen; modularize het vergemakkelijken van groter herbruikbaarheid van code. Elke opgeslagen procedure parameters zodat ze nog meer flexibele ook geaccepteerd.

SQL Data Warehouse biedt een opgeslagen procedure voor vereenvoudigde en gestroomlijnde implementatie. Het grootste verschil ten opzichte van SQL Server is dat de opgeslagen procedure niet vooraf gecompileerde code. In datawarehouses we over het algemeen minder betrokken zijn bij de compilatietijd. Het is belangrijk dat de opgeslagen procedurecode correct is geoptimaliseerd wanneer u met grote hoeveelheden gegevens. Het doel is om op te slaan, uren, minuten en seconden geen milliseconden. Daarom is het handiger om na te denken van opgeslagen procedures als containers voor SQL-logica.     

Wanneer SQL Data Warehouse de opgeslagen procedure voert zijn SQL-instructies geparseerd, vertaald en geoptimaliseerde tijdens runtime. Tijdens dit proces wordt elke instructie omgezet in gedistribueerde query's. De SQL-code die daadwerkelijk wordt uitgevoerd op de gegevens wordt om de query ingediend.

## <a name="nesting-stored-procedures"></a>Nesten van opgeslagen procedures
Wanneer opgeslagen procedures aanroepen van andere opgeslagen procedures of dynamische SQL-instructies uitvoeren en vervolgens de binnenste procedure of code aanroepen opgeslagen, wordt gezegd dat worden genest.

SQL Data Warehouse ondersteuning voor maximaal 8 geneste niveaus. Dit is iets anders met SQL Server. Het niveau van nesten in SQL Server is 32.

Het bovenste niveau opgeslagen procedure-aanroep is gelijk als u wilt nesten, niveau 1

```sql
EXEC prc_nesting
```
Als de opgeslagen procedure kan ook een andere EXEC roept vervolgens neemt hierdoor het nest-niveau 2
```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Als u de tweede procedure wordt vervolgens uitgevoerd sommige dynamische sql vervolgens vergroten dit door het nest-niveau 3
```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Opmerking SQL Data Warehouse biedt momenteel geen ondersteuning voor @@NESTLEVEL. U moet een om bij te houden van het niveau van het nest zelf. Het is onwaarschijnlijk dat u het nest 8 limiet wordt bereikt, maar als u dat doet moet u opnieuw uw code en 'samenvoegen' dat zodat deze binnen deze termijn past.

## <a name="insertexecute"></a>INVOEGEN... UITVOEREN
SQL Data Warehouse staat niet toe dat u de resultaatset van een opgeslagen procedure met een INSERT-instructie gebruiken. Er is echter een alternatieve methode die u kunt gebruiken.

Raadpleeg het volgende artikel op [tijdelijke tabellen] voor een voorbeeld van hoe u dit doet.

## <a name="limitations"></a>Beperkingen

Er zijn enkele aspecten van Transact-SQL opgeslagen procedures die niet zijn geïmplementeerd in SQL Data Warehouse.

Deze zijn:

- tijdelijke opgeslagen procedures
- genummerde opgeslagen procedures
- uitgebreide opgeslagen procedures
- CLR opgeslagen procedures
- de optie codering
- replicatie-optie
- tabelwaarden parameters
- alleen-lezen-parameters
- standaardparameters
- contexten worden uitgevoerd
- Return, instructie

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[tijdelijke tabellen]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
