<properties
    pageTitle="In Memory OLTP SQL txn perf verbetert | Microsoft Azure"
    description="Hanteer In Memory OLTP transactionele prestaties in een bestaande SQL-database te verbeteren."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor="MightyPen"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="use-in-memory-oltp-preview-to-improve-your-application-performance-in-sql-database"></a>Gebruik In Memory OLTP (voorbeeld) voor het verbeteren van de toepassingsprestaties van uw in SQL-Database

[In Memory OLTP](sql-database-in-memory.md) kan worden gebruikt voor het verbeteren van werkbelasting in [Premium](sql-database-service-tiers.md) Azure SQL Databases OLTP zonder verhoging van het prestatieniveau van de.

Volg deze stappen vast te stellen In Memory OLTP in uw bestaande database.

## <a name="step-1-ensure-your-premium-database-supports-in-memory-oltp"></a>Stap 1: Controleer dat uw database Premium ondersteunt In Memory OLTP

Premium-databases die zijn gemaakt in November 2015 of later, ondersteunen de functie In-Memory. U kunt nagaan of uw database premie de In-Memory-functie ondersteunt door de volgende Transact-SQL-instructie uit te voeren. In het geheugen wordt ondersteund als het geretourneerde resultaat 1 (niet 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* staat voor *Extreme Transaction Processing*

Als uw bestaande database moet worden verplaatst naar een nieuwe V12 Premium-database, kunt u de volgende technieken om de gegevens importeren en exporteren.

#### <a name="export-steps"></a>Stappen te exporteren

Uw productiedatabase exporteren naar een bacpac met behulp van:

- De functie [exporteren](sql-database-export.md) in de [portal](https://portal.azure.com/).

- De **gegevenslaag van Export** -functionaliteit in een [recente SSMS.exe](http://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio).
 1. Vouw het knooppunt **Databases** in **Object Explorer**.
 2. Met de rechtermuisknop op het knooppunt van uw database.
 3. Klik op **taken** > **- gegevenslaag toepassing exporteren**.
 4. Werken in het venster van de wizard die wordt weergegeven.


#### <a name="import-steps"></a>Importstappen

De bacpac in een nieuwe Premium-database importeren.

1. In het [portal](https://portal.azure.com/)voor Azure
 - Ga naar de server.
 - Selecteer de optie [Database importeren](sql-database-import.md) .
 - Selecteer een Premium prijzen laag.

2. SSMS de bacpac importeren gebruiken:
 - In **Object Explorer**met de rechtermuisknop op het knooppunt **Databases** uit.
 - Klik op **Import Data-Tier-toepassing**.
 - Werken in het venster van de wizard die wordt weergegeven.


## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Stap 2: Bepaal objecten om te migreren naar In Memory OLTP

SSMS bevat een rapport **Prestaties analyseren-overzicht transactie** die u op een database met een actieve werkbelasting uitvoeren kunt. Het rapport bevat de tabellen en opgeslagen procedures die kandidaten voor de migratie naar In Memory OLTP zijn.

In SSMS om het rapport te genereren:
- In **Object Explorer**met de rechtermuisknop op het knooppunt van uw database.
- Klik op **rapporten** > **standaardrapporten** > **transactie prestaties analyseren-overzicht**.

Zie [vaststellen als een tabel of een opgeslagen Procedure moet worden overgezet naar de In Memory OLTP](http://msdn.microsoft.com/library/dn205133.aspx)voor meer informatie.


## <a name="step-3-create-a-comparable-test-database"></a>Stap 3: Maak een vergelijkbare testdatabase

Stel dat de lijst wordt aangegeven dat de database bevat een tabel die wordt geconverteerd naar een tabel geheugen geoptimaliseerd zou komen. Het is raadzaam dat u eerst testen om te bevestigen de aanwijzing door te testen.

U moet een kopie van de test van de productiedatabase. De testdatabase moet op dezelfde laag serviceniveau als uw productiedatabase.

Om te vereenvoudigen testen, tweak u uw testdatabase als volgt:

1. Verbinding maken met de testdatabase met behulp van SSMS.

2. Om te voorkomen dat de optie met (MOMENTOPNAME) in query's, stelt u de databaseoptie, zoals in de volgende T-SQL-instructie:
```
ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```


## <a name="step-4-migrate-tables"></a>Stap 4: Tabellen migreren

U moet maken en vullen van een geoptimaliseerde geheugen kopie van de tabel die u wilt testen. U kunt deze maken met behulp van:

- De handige geheugen Wizard optimalisatie in SSMS.
- Handmatige T-SQL.


#### <a name="memory-optimization-wizard-in-ssms"></a>Geheugen optimalisatie Wizard in SSMS

Gebruik deze migratieoptie:

1. Verbinding maken met de testdatabase met SSMS.

2. In **Object Explorer**met de rechtermuisknop op de tabel en klik vervolgens op **Geheugen optimalisatie Advisor**.
 - De wizard **Tabel geheugen Optimizer Advisor** wordt weergegeven.

3. Klik in de wizard **migratie validatie** (of de knop **volgende** ) of de tabel heeft een niet-ondersteunde functies die niet worden ondersteund in tabellen geheugen wordt geoptimaliseerd. Zie voor meer informatie:
 - De *Controlelijst van geheugen optimaliseren* in [Geheugen optimalisatie Advisor](http://msdn.microsoft.com/library/dn284308.aspx).
 - [Transact-SQL-constructies worden niet ondersteund door OLTP In het geheugen](http://msdn.microsoft.com/library/dn246937.aspx).
 - [Migreren naar In Memory OLTP](http://msdn.microsoft.com/library/dn247639.aspx).

4. Als de tabel geen niet-ondersteunde functies, kan de adviseur het werkelijke schema en gegevensmigratie voor u uitvoeren.


#### <a name="manual-t-sql"></a>Handmatige T-SQL

Gebruik deze migratieoptie:

1. Verbinding maken met uw testdatabase met behulp van SSMS (of een vergelijkbaar hulpprogramma).

2. Haal het volledige T-SQL-script voor de tabel en de bijbehorende indexen.
 - SSMS, met de rechtermuisknop op het knooppunt van de tabel.
 - Klik op **de Scripttabel als** > **maken om te** > **nieuwe Query-venster**.

3. Voeg in het script-venster met (MEMORY_OPTIMIZED = ON) met de instructie CREATE TABLE.

4. Als u een GECLUSTERDE index, wijzigen in NONCLUSTERED.

5. Naam van de bestaande tabel wijzigen met behulp van SP_RENAME.

6. De nieuwe geheugen geoptimaliseerd kopie maken van de tabel door uw bewerkte CREATE TABLE-script uit te voeren.

7. De gegevens kopiëren naar de tabel geheugen geoptimaliseerd met behulp van invoegen... SELECTEER * IN:
    
```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Stap 5 (optioneel): opgeslagen procedures migreren

Een opgeslagen procedure voor betere prestaties kunt ook wijzigen door de functie In-Memory.


### <a name="considerations-with-natively-compiled-stored-procedures"></a>Overwegingen met native gecompileerde opgeslagen procedures

Een zelf gecompileerde opgeslagen procedure moet op de component T-SQL met de volgende opties:

- NATIVE_COMPILATION

- TOEPASSING: wat betekent dat tabellen met de opgeslagen procedure kan niet de kolomdefinities gewijzigd op een manier die invloed zou hebben op de opgeslagen procedure, tenzij u de opgeslagen procedure weghalen.


Een systeemeigen module moet één grote [blokken ATOMAIRE](http://msdn.microsoft.com/library/dn452281.aspx) gebruiken voor beheer van transacties. Er is geen functie voor een expliciete BEGIN TRANSACTION of ROLLBACK TRANSACTION. Als uw code een schending van een zakelijke regel detecteert, kan deze de atomaire blok met een instructie [THROW](http://msdn.microsoft.com/library/ee677615.aspx) eindigen.


### <a name="typical-create-procedure-for-natively-compiled"></a>Normale PROCEDURE maken voor zichzelf gecompileerd

Meestal is de T-SQL voor het maken van een standaard gecompileerde opgeslagen procedure vergelijkbaar met de volgende sjabloon:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

- MOMENTOPNAME is voor de TRANSACTION_ISOLATION_LEVEL, de meest voorkomende waarde voor het standaard gecompileerde opgeslagen procedure. Een subset van de andere waarden zijn, worden ook ondersteund:
 - HERHAALBARE LEESBEWERKING
 - SERIALISATIE


- De waarde van de taal moet aanwezig zijn in de weergave van de sys.languages.


### <a name="how-to-migrate-a-stored-procedure"></a>Het migreren van een opgeslagen procedure

De migratiestappen zijn:


1. Het script voor CREATE PROCEDURE aan de opgeslagen procedure met regelmatige geïnterpreteerde krijgen.

2. Herschrijf de koptekst in de vorige sjabloon aan.

3. Gaan of de opgeslagen procedure T-SQL-code gebruikmaakt van een van de functies die niet worden ondersteund voor native gecompileerde opgeslagen procedures. Toepassen van de oplossingen indien nodig.
 - Zie [Migratiekwesties voor native gecompileerd opgeslagen Procedures](http://msdn.microsoft.com/library/dn296678.aspx)voor meer informatie.

4. Wijzig de naam van de oude, opgeslagen procedure met behulp van SP_RENAME. Of u gewoon.

5. Het bewerkte CREATE PROCEDURE T-SQL-script uitvoert.


## <a name="step-6-run-your-workload-in-test"></a>Stap 6: Uw werkbelasting in test uitvoeren

Een werkbelasting in uw testdatabase die lijkt op de belasting die wordt uitgevoerd in de productiedatabase uitgevoerd. Dit moet de prestatieverbetering bereikt door het gebruik van de In-Memory-functie voor tabellen en opgeslagen procedures te onthullen.

Belangrijke kenmerken van de werkbelasting zijn:

- Het aantal gelijktijdige verbindingen.

- Verhouding tussen het lezen/schrijven.


Als u wilt aanpassen en de werkbelasting van de test uitvoert, kunt u overwegen het hulpprogramma handige ostress.exe, die in [hier](sql-database-in-memory.md)wordt geïllustreerd.


Om te minimaliseren netwerkvertraging, de test worden uitgevoerd in dezelfde Azure geografische regio waar de database zich bevindt.


## <a name="step-7-post-implementation-monitoring"></a>Stap 7: Na de implementatie monitoring

Neem de controle van de gevolgen van de prestaties van uw implementaties In het geheugen van de productie:

- [Monitor geheugenopslag](sql-database-in-memory-oltp-monitoring.md).

- [Azure SQL-Database met behulp van beheer van dynamische weergaven controleren](sql-database-monitoring-with-dmvs.md)


## <a name="related-links"></a>Verwante koppelingen

- [In Memory OLTP (In het geheugen optimalisatie)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Inleiding tot het standaard gecompileerde opgeslagen Procedures](http://msdn.microsoft.com/library/dn133184.aspx)

- [Geheugen optimalisatie Advisor](http://msdn.microsoft.com/library/dn284308.aspx)

