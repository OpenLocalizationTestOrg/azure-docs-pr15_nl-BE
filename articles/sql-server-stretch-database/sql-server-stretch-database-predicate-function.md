<properties
    pageTitle="Selecteer rijen te migreren met behulp van een filter-functie (Database uitrekken) | Microsoft Azure"
    description="Informatie over rijen te migreren met behulp van de filterfunctie van een selecteren."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="douglasl"/>

# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Selecteer rijen te migreren met behulp van een filter-functie (uitrekken Database)

Als u koude gegevens in een afzonderlijke tabel opslaat, kunt u Database uitrekken om te migreren van de gehele tabel. Als uw tabel gegevens van warm en koud bevat, aan de andere kant kunt u een filterfunctie selecteert u de rijen wilt migreren. Het predikaat filter is een tabel in line\-functie gewaardeerd. Dit onderwerp wordt beschreven hoe u een tabel in line\-waarde van de functie voor het selecteren van rijen te migreren.

>   [AZURE.NOTE] Als u een filterfunctie niet goed werkt, werkt gegevensmigratie ook niet goed. Uitrekken Database geldt de filter-functie voor de tabel met de operator CROSS APPLY.

Als u een filter-functie niet opgeeft, wordt de hele tabel wordt gemigreerd.

Wanneer u de Database inschakelen voor uitrekken Wizard uitvoert, kunt u een hele tabel migreren of u kunt een functie eenvoudig filter opgeven in de wizard. Als u een ander type filter-functie gebruiken om rijen te migreren selecteren, doet u een van de volgende dingen.

-   De wizard af te sluiten en uitvoeren van de instructie ALTER TABLE uitrekken inschakelen voor de tabel en een filter-functie opgeven.

-   Uitvoeren van de instructie ALTER TABLE een filterfunctie opgeven nadat u de wizard af te sluiten.

De syntaxis ALTER TABLE voor het toevoegen van een functie wordt verderop in dit onderwerp beschreven.

## <a name="basic-requirements-for-the-filter-function"></a>Basisvereisten voor de filter-functie
De tabel in line\-gewaardeerde functie die vereist is voor een predikaat uitrekken Database filter ziet eruit als in het volgende voorbeeld.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE <predicate>
```
De parameters voor de functie hebben-id's voor de kolommen uit de tabel niet.

Schemabinding is vereist om te voorkomen dat de kolommen die worden gebruikt door de filterfunctie worden verwijderd of gewijzigd.

### <a name="return-value"></a>Retourwaarde
Als de functie resulteert in een niet\-leeg resultaat de rij komt in aanmerking moeten worden gemigreerd. Anders \- dat wil zeggen, als de functie geen resultaat oplevert \- de rij komt niet in aanmerking moeten worden gemigreerd.

### <a name="conditions"></a>Voorwaarden
De &lt; *predicaat* &gt; kan bestaan van één voorwaarde of voorwaarden gekoppeld met de logische operator en.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Elke voorwaarde die kan op zijn beurt bestaan van een primitieve voorwaarde of primitieve voorwaarden gekoppeld met de logische operator of.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### <a name="primitive-conditions"></a>Primitieve omstandigheden
Een primitieve voorwaarde kan een van de volgende vergelijkingen kunt doen.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Een parameter met een constante expressie van de functie vergelijken. Bijvoorbeeld `@column1 < 1000`.

    Hier volgt een voorbeeld waarin wordt gecontroleerd of de waarde van een kolom *datum* &lt; 1/1/2016.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   De operator IS NULL of IS NOT NULL toepassen op een parameter van de functie.

-   De operator in gebruiken om een parameter van de functie om een lijst met constante waarden te vergelijken.

    Hier volgt een voorbeeld dat controleert of de waarde van een *verzending\_status* kolom is `IN (N'Completed', N'Returned', N'Cancelled')`.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

### <a name="comparison-operators"></a>Vergelijkingsoperatoren
De volgende vergelijkingsoperators worden ondersteund.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### <a name="constant-expressions"></a>Constante expressies
De constanten die u kunt in de filterfunctie van een gebruiken kunnen worden deterministic expressie kan worden geëvalueerd wanneer u de functie definieert. Constante expressies kunnen de volgende zaken bevatten.

-   Letterlijke waarden. Bijvoorbeeld `N’abc’, 123`.

-   Algebraïsche uitdrukkingen. Bijvoorbeeld `123 + 456`.

-   Deterministische functies. Bijvoorbeeld `SQRT(900)`.

-   Deterministic conversies die CAST of CONVERT gebruiken. Bijvoorbeeld `CONVERT(datetime, '1/1/2016', 101)`.

### <a name="other-expressions"></a>Andere expressies
Kunt u het niet tussen operatoren en tussen als de resulterende functie aan de regels die hier beschreven voldoet nadat u de BETWEEN vervangen en niet tussen exploitanten met de expressies equivalent en en of.

U kunt geen subquery's gebruiken of niet\-deterministische functies zoals ASELECT() of GETDATE().

## <a name="add-a-filter-function-to-a-table"></a>De filterfunctie van een aan een tabel toevoegen
Filter-functie toevoegen aan een tabel door met de instructie **ALTER TABLE** en opgeven van een bestaande tabel met in line\-functie gewaardeerd als de waarde van de **FILTER\_PREDIKAAT** parameter. Bijvoorbeeld:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
) )
```
Nadat u de functie aan de tabel als een predicaat koppelt, wordt de volgende dingen waar zijn.

-   De volgende keer data migratie plaatsvindt, alleen de rijen waarvoor de functie resulteert in een niet\-lege waarde worden gemigreerd.

-   De kolommen die worden gebruikt door de functie zijn schema gebonden. U kunt deze kolommen niet wijzigen als een tabel is met behulp van de functie als de filter-predicaat.

U kunt de tabel in line niet neerzetten\-functie gewaardeerd als een tabel is met behulp van de functie als de filter-predicaat.

>   [AZURE.NOTE] Ter verbetering van de prestaties van de filter-functie, een index te maken voor de kolommen die door de functie wordt gebruikt.

### <a name="passing-column-names-to-the-filter-function"></a>Kolomnamen worden doorgegeven aan de filter-functie
Wanneer u een filterfunctie aan een tabel toewijst, geef de namen van de doorgegeven aan de filterfunctie met een naam van een onderdeel. Als u een driedelig naam als u de kolom namen, daaropvolgende query's op de Stretch\-tabel ingeschakelde mislukt.

Als u een driedelig kolomnaam opgeeft zoals in het volgende voorbeeld wordt getoond, de instructie wordt uitgevoerd maar mislukt de daaropvolgende query's op de tabel.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Geef de filter-functie met de kolomnaam van een van een onderdeel zoals in het volgende voorbeeld.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="addafterwiz"></a>Filter-functie toevoegen na het uitvoeren van de Wizard  

Als u een functie gebruiken die u maken in de Wizard **Database inschakelen voor uitrekken wilt kunt** , kunt u de instructie ALTER TABLE een functie opgeven nadat u de wizard afsluiten als u wilt uitvoeren. Voordat u een functie wilt toepassen, maar moet u stoppen van de gegevensmigratie die al wordt uitgevoerd en gemigreerde gegevens terughalen. (Klik voor meer informatie over waarom dit nodig is, Zie [vervangen door een bestaande filterfunctie](#replacePredicate).  

1. Omkeren van de richting van de migratie en de gegevens al gemigreerd terug te brengen. U kunt deze bewerking niet annuleren nadat deze is gestart. U ook kosten op Azure voor uitgaande gegevensoverdracht \(egress\). Zie voor meer info [hoe Azure prijzen werken](https://azure.microsoft.com/pricing/details/data-transfers/).  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Wacht totdat de migratie te voltooien. U kunt de status controleren in **Databasemonitor uitrekken** van SQL Server Management Studio, of u kunt de weergave van de **sys.dm_db_rda_migration_status** opvragen. Zie voor meer info [Monitor en gegevensmigratie problemen met](sql-server-stretch-database-monitor.md) of [sys.dm_db_rda_migration_status](https://msdn.microsoft.com/library/dn935017.aspx).  

3. Maak de filterfunctie die u wilt toepassen op de tabel.  

4. De functie toevoegen aan de tabel en gegevensmigratie naar Azure start.  

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## <a name="filter-rows-by-date"></a>Rijen filteren op datum
In het volgende voorbeeld worden de rijen waarin de kolom **datum** een waarde vóór 1 januari 2016 bevat gemigreerd.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## <a name="filter-rows-by-the-value-in-a-status-column"></a>Rijen filteren door de waarde in een statuskolom
In het volgende voorbeeld worden de rijen waarin de kolom **status** een van de opgegeven waarden bevat gemigreerd.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## <a name="filter-rows-by-using-a-sliding-window"></a>Rijen filteren met behulp van een schuifvenster
Rijen filteren met behulp van een schuifvenster, houd rekening met de volgende eisen van de filter-functie.

-   De functie heeft deterministisch. U kunt een functie die automatisch herberekenen van het schuifvenster verloop van tijd dus niet maken.

-   De schemabinding wordt gebruikt. Dus kunt niet je eenvoudigweg de functie 'in de plaats"elke dag door het aanroepen van de **Functie wijzigen** als u wilt het schuifvenster te verplaatsen.

Beginnen met een filter-functie zoals in het volgende voorbeeld, dat de rijen waarin de kolom **systemEndTime** een vóór 1 januari 2016 bevat worden gemigreerd.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

De filterfunctie toepassen op de tabel.

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Als u het schuifvenster bijwerken wilt, kunt u het volgende doen:

1.  Maak een nieuwe functie waarmee de nieuwe schuifvenster. Het volgende voorbeeld wordt de datums vóór 2 januari 2016 in plaats van 1 januari 2016.

2.  Het vorige filter-functie vervangen door de nieuwe door de aanroepende **ALTER TABLE**, zoals in het volgende voorbeeld.

3. Het vorige filter-functie die u niet meer gebruikt door de aanroepende **Functie neerzetten**desgewenst verwijderen. (Deze stap wordt niet weergegeven in het voorbeeld.)

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as the filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## <a name="more-examples-of-valid-filter-functions"></a>Meer voorbeelden van geldige filterfuncties

-   In het volgende voorbeeld worden twee primitieve omstandigheden gecombineerd met behulp van de logische operator en.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Het volgende voorbeeld worden de verschillende voorwaarden en een deterministische conversie met converteren.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   In het volgende voorbeeld wordt de wiskundige operatoren en functies.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   In het volgende voorbeeld wordt het niet tussen operatoren en tussen. Dit gebruik is geldig, omdat de resulterende functie aan de regels die hier beschreven voldoet nadat u de BETWEEN vervangen en niet tussen exploitanten met de expressies equivalent en en of.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 BETWEEN 0 AND 100
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    De voorgaande functie komt overeen met de volgende functie nadat u vervangen door de niet tussen operatoren en tussen de expressies equivalent en en of.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## <a name="examples-of-filter-functions-that-arent-valid"></a>Voorbeelden van filterfuncties die niet geldig

-   De volgende functie is niet geldig omdat het bevat een niet\-deterministische conversie.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   De volgende functie is niet geldig omdat het bevat een niet\-deterministisch functie-aanroep.

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   De volgende functie is niet geldig omdat het een subquery bevat.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   De volgende functies zijn niet geldig omdat expressies die algebraïsche operators of ingebouwde\-in functies moet resulteren in een constante wanneer u de functie definieert. U kunt geen kolomverwijzingen opnemen in algebraïsche uitdrukkingen of functieaanroepen.

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE SQRT(@column1) = 30
    GO
    ```

-   De volgende functie is niet geldig omdat het in strijd met de regels die hier beschreven nadat u de operator BETWEEN door de equivalente en expressie vervangen.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    De voorgaande functie komt overeen met de volgende functie nadat u de operator BETWEEN door de equivalente en expressie vervangen. Deze functie is niet geldig omdat de primitieve omstandigheden kunnen alleen de logische operator OR gebruiken.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## <a name="how-stretch-database-applies-the-filter-function"></a>Hoe Database uitrekken van toepassing is de filter-functie
Uitrekken Database de filterfunctie toegepast op de tabel en in aanmerking komende rijen bepaalt met behulp van de operator CROSS APPLY. Bijvoorbeeld:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Als de functie resulteert in een niet\-leeg resultaat voor de rij, de rij komt in aanmerking moeten worden gemigreerd.

## <a name="replacePredicate"></a>Vervang een bestaande filterfunctie
Kunt u een functie eerder opgegeven filter vervangen door de instructie **ALTER TABLE** opnieuw uitvoeren en een nieuwe waarde voor de **FILTER\_PREDIKAAT** parameter. Bijvoorbeeld:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
```
De nieuwe tabel van de in line\-gewaardeerde functie heeft de volgende eisen voldoen.

-   De nieuwe functie is minder beperkend zijn dan de vorige functie.

-   De operatoren die bestond in de oude functie moeten bestaan in de nieuwe functie.

-   De nieuwe functie kan geen operators die niet in de oude functie bevatten.

-   De volgorde van de operator argumenten wijzigen niet.

-   Alleen constante waarden die deel van uitmaken een `<, <=, >, >=` vergelijking kan worden gewijzigd op een manier waardoor de functie minder beperkend zijn.

### <a name="example-of-a-valid-replacement"></a>Voorbeeld van een geldige vervanging
Stel dat de volgende functie het huidige filter predikaat wordt.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
De volgende functie is een geldige vervanging omdat de nieuwe datumconstante (dat geeft een hoger afsluitdatum) de functie minder beperkend zijn maakt.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
            AND (@column2 < -50 OR @column2 > 50)
GO
```

### <a name="examples-of-replacements-that-arent-valid"></a>Voorbeelden van vervangingen die niet geldig
De volgende functie is een geldige vervanging niet omdat de nieuwe datumconstante (waarmee een eerder afsluitdatum) niet de functie minder beperkend maakt.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
De volgende functie is een geldige vervanging niet omdat een van de vergelijkingsoperators is verwijderd.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -50)
GO
```
De volgende functie is een geldige vervanging niet omdat er is een nieuwe voorwaarde toegevoegd met de logische operator en.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
            AND (@column2 <> 0)
GO
```

## <a name="remove-a-filter-function-from-a-table"></a>De filterfunctie van een uit een tabel verwijderen
De migratie van de hele tabel in plaats van de geselecteerde rijen, door de bestaande functie te verwijderen door de instelling **FILTER\_PREDIKAAT** op null. Bijvoorbeeld:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = NULL,
    MIGRATION_STATE = <desired_migration_state>
) )
```
Nadat u de filterfunctie verwijdert, worden alle rijen in de tabel gemigreerd. Hierdoor kunt u niet een filterfunctie voor dezelfde tabel later opgeven tenzij u de externe gegevens voor de tabel van Azure eerst brengen. Deze beperking bestaat om te voorkomen dat de situatie waarin rijen die niet gemigreerd zijn als u een nieuw filter-functie biedt al zijn gemigreerd naar Azure.

## <a name="check-the-filter-function-applied-to-a-table"></a>De filterfunctie is toegepast op een tabel controleren
Controleer de filterfunctie is toegepast op een tabel, opent u de catalogusweergave **sys.remote\_gegevens\_archief\_tabellen** en controleert u de waarde van de **filter\_predicaat** kolom. Als de waarde null is, komt de gehele tabel in aanmerking voor archivering. Zie voor meer info, [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="security-notes-for-filter-functions"></a>Opmerkingen over beveiliging voor filterfuncties  
Een account die oneigenlijk met db_owner bevoegdheden kan de volgende dingen doen.  

-   Maken en toepassen van een tabelwaardefunctie die grote hoeveelheden server resources verbruikt of wacht voor een langere periode, wat resulteert in een denial of service.  

-   Maken en toepassen van een tabelwaardefunctie waardoor de inhoud van een tabel waarvoor de gebruiker heeft zijn expliciet de toegang lezen afleiden.  

## <a name="see-also"></a>Zie ook

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
