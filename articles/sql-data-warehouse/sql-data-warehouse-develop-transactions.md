<properties
   pageTitle="Transacties in SQL datawarehouse | Microsoft Azure"
   description="Tips voor het implementeren van transacties in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="transactions-in-sql-data-warehouse"></a>Transacties in SQL datawarehouse

Zoals u verwachten zou, ondersteunt SQL Data Warehouse transacties als onderdeel van de werkbelasting van het magazijn gegevens. Ervoor te zorgen de prestaties van SQL Data Warehouse op schaal zijn sommige functies echter beperkt in vergelijking met SQL Server. In dit artikel worden de verschillen gemarkeerd en geeft een overzicht van de andere. 

## <a name="transaction-isolation-levels"></a>Transactie-isolatieniveaus
SQL Data Warehouse implementeert zuur transacties. De isolatie van de transactionele ondersteuning is echter beperkt tot `READ UNCOMMITTED` en dit kan niet worden gewijzigd. U kunt een aantal methoden om te voorkomen dat ongeldige waarden van gegevens als dit een probleem voor u codering implementeren. De meest populaire methoden benutten CTAS en tabel partitie overschakelen naar een andere (vaak de verschuivende venster patroon genoemd) om te voorkomen dat gebruikers bij het controleren van gegevens die nog steeds wordt voorbereid. Weergaven die vooraf filteren van gegevens is ook een populaire aanpak.  

## <a name="transaction-size"></a>Grootte van de transactie
Een transactie gegevens één wijziging slechts een beperkte grootte. De limiet is vandaag "per distributie" toegepast. Daarom kan de totale toewijzing worden berekend door te vermenigvuldigen met de limiet voor het aantal distributie. Om bij benadering het maximum aantal rijen in de transactie verdelen de lampvoet distributie door de totale grootte van elke rij. Rekening houden met een gemiddelde kolomlengte nemen in plaats van met behulp van de maximale grootte voor kolommen met variabele lengte.

In de tabel onder de volgende veronderstellingen zijn aangebracht:

* Een gelijke verdeling van de gegevens is opgetreden. 
* De gemiddelde rijlengte is 250 bytes

| [DWU][]    | Cap per distributie (verwijderen) | Aantal distributies | Maximumgrootte van transactie (verwijderen) | # Aantal rijen per distributie | Maximum aantal rijen per transactie |
| ------ | -------------------------- | ----------------------- | -------------------------- | ----------------------- | ------------------------ |
| DW100  |  1                         | 60                      |   60                       |   4,000,000             |    240,000,000           |
| DW200  |  1.5                       | 60                      |   90                       |   6,000,000             |    360,000,000           |
| DW300  |  2,25                      | 60                      |  135                       |   9,000,000             |    540,000,000           |
| DW400  |  3                         | 60                      |  180                       |  12,000,000             |    720,000,000           |
| DW500  |  3,75                      | 60                      |  225                       |  15,000,000             |    900,000,000           |
| DW600  |  4.5                       | 60                      |  270                       |  18,000,000             |  1,080,000,000           |
| DW1000 |  7.5                       | 60                      |  450                       |  30,000,000             |  1,800,000,000           |
| DW1200 |  9                         | 60                      |  540                       |  36,000,000             |  2,160,000,000           |
| DW1500 | 11,25                      | 60                      |  675                       |  45,000,000             |  2,700,000,000           |
| DW2000 | 15                         | 60                      |  900                       |  60.000.000             |  3,600,000,000           |
| DW3000 | 22.5                       | 60                      |  1,350                     |  90,000,000             |  5,400,000,000           |
| DW6000 | 45                         | 60                      |  2700                     | 180,000,000             | 10,800,000,000           |

De limiet voor de transactie wordt per transactie of bewerking toegepast. Niet wordt toegepast op alle gelijktijdige transacties. Elke transactie is daarom toegestaan deze hoeveelheid gegevens naar het logboek schrijven. 

Om te optimaliseren en het minimaliseren van de hoeveelheid gegevens in het logboek opgenomen raadpleegt u het artikel met [Aanbevolen procedures voor transacties][] .

> [AZURE.WARNING] De grootte van de maximale transactie kan alleen worden bereikt voor de HASH of zelfs ROUND_ROBIN distributed tabellen waarin de verspreiding van de gegevens is. Als de transactie gegevens op asymmetrische wijze naar de verdelingen schrijft zijn de limiet is waarschijnlijk worden voorafgaand aan de transactie op maximale grootte bereikt.
<!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>Transactiestadium
SQL Data Warehouse wordt de functie XACT_STATE() gebruikt voor aangifte van een mislukte transactie met behulp van de waarde -2. Dit betekent dat de transactie is mislukt en is gemarkeerd voor alleen ongedaan maken

> [AZURE.NOTE] Het gebruik van -2 door de functie XACT_STATE om aan te geven een mislukte transactie vertegenwoordigt verschillende probleem met SQL Server. SQL Server gebruikt de waarde -1 voor een transactie niet committable. SQL Server kan bepaalde fouten binnen een transactie zonder worden gemarkeerd als niet committable tolereren. Bijvoorbeeld `SELECT 1/0` wordt een fout veroorzaken, maar niet dwingen een transactie in een niet-committable staat. SQL Server kunnen ook gelezen in de transactie niet committable. Echter kunt SQL Data Warehouse u dit doen. Als een fout binnen een transactie SQL Data Warehouse optreedt treedt automatisch de status-2 en worden niet Breng een of meer select-instructies totdat de instructie is teruggedraaid. Het is daarom belangrijk om te controleren dat uw toepassingscode om te zien als XACT_STATE() wordt gebruikt als u moet code wijzigingen aanbrengen.

In SQL Server ziet u mogelijk een transactie die er als volgt uit:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Als u uw code niet als deze hoger dan is krijgt u het volgende foutbericht weergegeven:

Msg 111233, niveau 16, staat 1, regel 1 111233; De huidige transactie is afgebroken en de wachtende wijzigingen zijn teruggedraaid. Oorzaak: Een transactie alleen terugdraaien-status is niet expliciet teruggedraaid voordat een DDL-, DML- of SELECT-instructie.

U krijgt geen ook de uitvoer van de ERROR_ *-functies.

De code moet worden enigszins gewijzigd in SQL Data Warehouse:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Het verwachte gedrag is nu waargenomen. De fout in de transactie wordt beheerd en de ERROR_ *-functies kunt u waarden zoals verwacht.

Dat is gewijzigd, is dat de `ROLLBACK` van de transactie moest gebeuren voor het lezen van de gegevens in de `CATCH` blokkeren.

## <a name="errorline-function"></a>Error_Line(), functie
Het is ook vermeldenswaardig dat SQL Data Warehouse niet implementeren of de functie ERROR_LINE() ondersteunen. Als u in uw code die u moet verwijderen om te voldoen aan de SQL Data Warehouse hebt. Query-labels in uw code gebruiken om vergelijkbare functionaliteit te implementeren. Raadpleeg het artikel [LABEL][] voor meer informatie over deze functie.

## <a name="using-throw-and-raiserror"></a>Met behulp van WEGGOOIEN en RAISERROR
WEGGOOIEN is de moderne uitvoering voor het verhogen van de uitzonderingen in SQL Data Warehouse maar RAISERROR wordt ook ondersteund. Er zijn een paar verschillen die zijn waard Let maar op.

- Door de gebruiker gedefinieerde foutberichten getallen kan niet in het bereik 100.000 150.000 voor WEGGOOIEN
- RAISERROR foutberichten worden vastgesteld op 50.000
- Gebruik van sys.messages wordt niet ondersteund.

## <a name="limitiations"></a>Limitiations
SQL Data Warehouse heeft een paar andere beperkingen die betrekking op transacties hebben.

Ze zijn als volgt:

- Geen gedistribueerde transacties
- Geen geneste transacties dat is toegestaan
- Nee, behalve punten toegestaan
- Geen benoemde transacties
- Geen gemarkeerde transacties
- Geen ondersteuning voor DDL zoals `CREATE TABLE` binnen een gebruiker gedefinieerd transactie

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het optimaliseren van transacties, de [Aanbevolen procedures voor transacties][].  Meer informatie over andere aanbevelingen SQL Data Warehouse, Zie [Aanbevolen procedures voor SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[development overview]: ./sql-data-warehouse-overview-develop.md
[Aanbevolen procedures voor transacties]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Aanbevolen procedures voor SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
