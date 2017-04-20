 <properties
    pageTitle="Batchverwerking gebruiken voor het verbeteren van toepassingsprestaties Azure SQL-Database"
    description="Het onderwerp het bewijs levert dat batchen databasebewerkingen aanzienlijk imroves de snelheid en de schaalbaarheid van uw toepassingen Azure SQL-Database. Hoewel deze batchen technieken voor een SQL Server-database, wordt de focus van het artikel op Azure."
    services="sql-database"
    documentationCenter="na"
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="07/12/2016"
    ms.author="annemill" />

# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Batchverwerking gebruiken voor het verbeteren van de prestaties van de SQL-Database

Batchen bewerkingen met Azure SQL-Database aanzienlijk verbetert de prestaties en schaalbaarheid van uw toepassingen. Om te begrijpen van de voordelen, het eerste deel van dit artikel heeft betrekking op sommige controletestresultaten sequentiële en batch-aanvragen met een SQL-Database te vergelijken. De rest van het artikel bevat de technieken, scenario's en overwegingen om te gebruiken met succes in uw toepassingen Azure batchen.

**Auteurs**: Jason Roth, Silvano Coriani, Theo Swanson (volledige schaal 180 incl.)

**Revisoren**: Conor Cunningham, Michael Thomassy

## <a name="why-is-batching-important-for-sql-database"></a>Waarom is batchen belangrijk voor SQL-Database?
Oproepen naar een externe service batchen is een bekende strategie voor hogere prestaties en schaalbaarheid. Er zijn vaste interacties met een RAS-service, zoals serialisatie en netwerkoverdracht deserialisatie verwerking kosten. Veel afzonderlijke transacties in één batch, worden deze kosten geminimaliseerd.

In dit artikel willen we verschillende SQL-Database batchen van strategieën en scenario's te onderzoeken. Hoewel deze strategieën zijn ook van belang voor toepassingen voor bedrijven die gebruikmaken van SQL Server, zijn er verschillende redenen voor het gebruik van de batchverwerking voor SQL-Database te markeren:

- Is mogelijk nog groter netwerkvertraging toegang tot SQL-Database, met name als er krijgt u toegang tot SQL-Database buiten het datacenter met hetzelfde Microsoft Azure.
- De multitenant kenmerken van een SQL-Database betekent dat de efficiëntie van de data access laag is afgestemd op de algehele schaalbaarheid van de database. SQL-Database moet voorkomen dat elke één huurder/gebruiker database bronnen ten nadele van andere huurders belasten. In reactie op het gebruik van meer dan de vooraf gedefinieerde targets, SQL-Database doorvoer verminderen of reageren met uitzonderingen te beperken. Efficiëntie, zoals een batchverwerking, kunnen u meer werk op de SQL-Database voordat deze limiet is bereikt. 
- Batchverwerking werkt ook voor architecturen die gebruikmaken van meerdere databases (sharding). De efficiëntie van uw interactie met de eenheid voor elke database is nog steeds een belangrijke factor in de algehele schaalbaarheid. 

Een van de voordelen van het gebruik van SQL-Database is dat u niet voor het beheren van de servers die host zijn van de database. Deze beheerde infrastructuur ook betekent echter dat er anders denken over de optimalisatie van de database. U kunt niet meer ter verbetering van de infrastructuur van hardware- of database zoeken. Microsoft Azure regelt deze omgevingen. Het belangrijkste gebied dat u kunt bepalen, is de interactie van uw toepassing met de SQL-Database. Batchverwerking is een van deze optimalisaties. 

Het eerste deel van het papier verschillende batchen worden technieken behandeld voor .NET-toepassingen die gebruikmaken van SQL-Database. De laatste twee secties omvatten batchen richtlijnen en scenario's.

## <a name="batching-strategies"></a>Strategieën batchen

### <a name="note-about-timing-results-in-this-topic"></a>Opmerking over de resultaten van de timing in dit onderwerp
>[AZURE.NOTE] Resultaten zijn geen benchmarks maar zijn bedoeld voor het weergeven van de **relatieve prestaties**. Tijdsinstellingen zijn gebaseerd op een gemiddelde van ten minste 10 test wordt uitgevoerd. Bewerkingen zijn ingevoegd in een lege tabel. Deze tests zijn pre-V12 gemeten en ze niet noodzakelijk overeen met doorvoer die kunnen optreden in een database met behulp van de nieuwe [service tiers](sql-database-service-tiers.md)V12. Het relatieve voordeel van de techniek batchen moet ongeveer gelijk zijn.

### <a name="transactions"></a>Transacties
Het lijkt vreemd om te beginnen met een herziening van de batchverwerking door transacties te bespreken. Maar het gebruik van client-side transacties heeft een subtiele serverzijde batchen-effect dat de prestaties verbetert. En transacties kunnen worden toegevoegd met slechts een paar regels code, zodat ze bieden een snelle manier om de prestaties van opeenvolgende bewerkingen te verbeteren.

Houd rekening met de volgende C#-code met een reeks invoegen en bewerkingen op een eenvoudige tabel bijwerken.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

De volgende code in de ADO.NET opeenvolgend deze bewerkingen worden uitgevoerd.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
    
        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

De beste manier om deze code te optimaliseren is voor het implementeren van enige vorm van client-side batchen van deze oproepen. Maar er is een eenvoudige manier om de prestaties van deze code verhogen door gewoon de volgorde van de aanroepen in een transactie. Hier is dezelfde code die gebruikmaakt van een transactie.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();
    
        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }
    
        transaction.Commit();
    }

In beide voorbeelden worden daadwerkelijk transacties gebruikt. In het eerste voorbeeld is elke individuele oproep een impliciete transactie. In het tweede voorbeeld loopt een expliciete transactie alle gesprekken. Per de documentatie voor het [vooraf geschreven transactielogboek](https://msdn.microsoft.com/library/ms186259.aspx)logboekrecords op de schijf verwijderd wanneer de transactie is doorgevoerd. Door meer oproepen in een transactie, kan het schrijven naar het transactielogboek dus uitstellen totdat de transactie doorgevoerd is. In feite inschakelt u batchen voor schrijfbewerkingen naar het transactielogboek van de server.

De volgende tabel bevat enkele ad-hoc-testresultaten. De tests de sequentiële invoegt met en zonder transacties uitgevoerd. Voor meer perspectief, de eerste reeks tests uitgevoerd op afstand vanaf een laptop met de database in Microsoft Azure. De tweede set van tests uitgevoerd via een cloud service en database beide was opgeslagen in het datacenter met hetzelfde Microsoft Azure (West VS). De volgende tabel ziet de duur in milliseconden van opeenvolgende toevoegingen met en zonder transacties.

**On-Premises tot Azure**:

| Bewerkingen | Er is geen transactie (ms) | Transactie (ms) |
|---|---|---|
| 1 | 130 | 402 |
| 10 | 1208 | 1226 |
| 100 | 12662 | 10395 |
| 1000 | 128852 | 102917 |


**Azure-Azure (dezelfde datacenter)**:

| Bewerkingen | Er is geen transactie (ms) | Transactie (ms) |
|---|---|---|
| 1 | 21 | 26 |
| 10 | 220 | 56 |
| 100 | 2145 | 341 |
| 1000 | 21479 | 2756 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de resultaten van de timing in dit onderwerp](#note-about-timing-results-in-this-topic).

Op basis van de testresultaten van de vorige, onmiddellijke verpakking één bewerking in een transactie neemt af prestaties. Maar als u meer bewerkingen in één transactie, de prestatieverbetering meer wordt gemarkeerd. De prestaties verschil is ook duidelijker merkbaar bij alle bewerkingen in het datacenter Microsoft Azure plaatsvinden. De wachttijd van het gebruik van SQL-Database buiten het datacenter Microsoft Azure overshadows de prestatieverbetering van de transacties.

Hoewel het gebruik van transacties de prestaties verbeteren kan, blijven [observeren aanbevolen procedures voor transacties en verbindingen](https://msdn.microsoft.com/library/ms187484.aspx). De transactie zo kort mogelijk te houden, en sluit de databaseverbinding nadat het werk is voltooid. Het gebruik van de instructie in het vorige voorbeeld zorgt ervoor dat de verbinding is gesloten, wanneer het volgende codeblok is voltooid.

Het vorige voorbeeld kunt u een lokale transactie op ADO.NET met twee regels code toevoegen. Transacties bieden een snelle manier om de prestaties van de code waarmee u opeenvolgende invoegen, bijwerken en verwijderen. Echter voor de snelste prestaties kunt u de code verder geprofiteerd van client-side batchen zoals tabelwaarden parameters te wijzigen.

Zie voor meer informatie over transacties in ADO.NET, [Lokale transacties in ADO.NET](https://msdn.microsoft.com/library/vstudio/2k2hy99x.aspx).

### <a name="table-valued-parameters"></a>Tabelwaarden parameters
Door de gebruiker gedefinieerde tabeltypen ondersteunen parameters tabelwaarden als parameters in Transact-SQL-instructies, opgeslagen procedures en functies. Deze client-side batchen techniek kunt u meerdere rijen met gegevens in de parameter tabelwaarden verzenden. Voor het gebruik van tabelwaarden parameters moet u eerst een tabelweergavetype definiëren. De volgende Transact-SQL-instructie wordt een tabelweergavetype met de naam **MyTableType**gemaakt.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
 

In de code maakt u een **DataTable** met exact dezelfde namen en typen van het tabeltype. Deze **DataTable** doorgeven in een parameter in een tekstquery of opgeslagen procedure-aanroep. In het volgende voorbeeld wordt deze techniek:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }
    
        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);
                    
        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });
    
        cmd.ExecuteNonQuery();
    }

In het vorige voorbeeld het **SqlCommand** -object voegt u rijen uit een parameter tabelwaarden **@TestTvp**. De eerder gemaakte **DataTable** -object is toegewezen aan deze parameter met de methode **SqlCommand.Parameters.Add** . De prestaties aanzienlijk batchen van de invoegt in een gesprek over opeenvolgende toevoegingen worden vergroot.

In het vorige voorbeeld nog verder verbeteren, een opgeslagen procedure te gebruiken in plaats van een opdracht op basis van tekst. De volgende Transact-SQL-opdracht wordt een opgeslagen procedure waarmee u de parameter **SimpleTestTableType** tabelwaarden gemaakt.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Vervolgens wijzigt u de verklaring **SqlCommand** -object in het vorige voorbeeld het volgende.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

In de meeste gevallen hebben de tabelwaarden parameters gelijkwaardige of betere prestaties dan andere batchen technieken. Tabelwaarden parameters zijn vaak beter, omdat ze flexibeler dan de andere opties zijn. Andere technieken, zoals SQL massaal kopiëren, staan bijvoorbeeld alleen het invoegen van nieuwe rijen. Maar met de tabelwaarden parameters kunt u logica in de opgeslagen procedure om te bepalen welke rijen van de updates zijn en die zijn ingevoegd. Het tabeltype kan ook worden gewijzigd voor een kolom 'Bewerking' die aangeeft of de opgegeven rij moet worden ingevoegd, bijgewerkt of verwijderd.

De volgende tabel bevat de resultaten van ad hoc voor het gebruik van tabelwaarden parameters in milliseconden.

| Bewerkingen | On-Premises tot Azure (ms)  | Azure dezelfde datacenter (ms) |
|---|---|---|
| 1 | 124 | 32 |
| 10 | 131 | 25 |
| 100 | 338 | 51 |
| 1000 | 2615 | 382 |
| 10000 | 23830 | 3586 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de resultaten van de timing in dit onderwerp](#note-about-timing-results-in-this-topic).

De prestatieverbetering van de batchverwerking is meteen duidelijk. In de vorige test sequentieel nam 1000 bewerkingen 129 seconden buiten het datacenter en 21 seconden van binnen het datacenter. Maar met de parameters tabelwaarden 1000 bewerkingen alleen 2.6 seconden buiten het datacenter en 0,4 seconden binnen het datacenter.

Zie voor meer informatie over parameters tabelwaarden [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>SQL massaal kopiëren
SQL massaal kopiëren is een andere manier om grote hoeveelheden gegevens invoegen in een doeldatabase. .NET-toepassingen kunnen gebruiken van de klasse **SqlBulkCopy** bulk insert-bewerkingen uit te voeren. **SqlBulkCopy** lijkt in functie op het opdrachtregelprogramma, **Bcp.exe**of de Transact-SQL-instructie **BULK INSERT**. In het volgende voorbeeld ziet u hoe massaal kopiëren de rijen in de bron **DataTable**, tabel naar de doeltabel in SQL Server, MyTable.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Er zijn bepaalde gevallen waar massaal kopiëren heeft de voorkeur boven tabelwaarden parameters. Zie de vergelijkingstabel van tabelwaarden parameters versus BULK INSERT-bewerkingen in het onderwerp [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).

De testresultaten van de ad hoc-tonen de prestaties van de batchverwerking met de **SqlBulkCopy** in milliseconden.

| Bewerkingen | On-Premises tot Azure (ms)  | Azure dezelfde datacenter (ms) |
|---|---|---|
| 1 | 433 | 57 |
| 10 | 441 | 32 |
| 100 | 636 | 53 |
| 1000 | 2535 | 341 |
| 10000 | 21605 | 2737 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de resultaten van de timing in dit onderwerp](#note-about-timing-results-in-this-topic).

De parameters gebruiken tabelwaarden ver in kleinere batch-loop van de klasse **SqlBulkCopy** . **SqlBulkCopy** uitgevoerd, 12 – 31% sneller dan de tabelwaarden parameters voor het testen van 1000 en 10.000 rijen. **SqlBulkCopy** is een goede optie voor batch ingevoegd, zoals tabelwaarden parameters, met name wanneer vergeleken met de prestaties van de bewerkingen niet batch verwerkt.

Zie voor meer informatie over het massaal kopiëren in ADO.NET, [Bulkkopieerbewerkingen in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Meerdere rijen met parameters INSERT-instructies
Een alternatief voor kleine batches is invoeginstructie te maken om grote geparametriseerde die meerdere rijen worden ingevoegd. In het volgende voorbeeld wordt deze techniek gedemonstreerd.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";
    
        SqlCommand cmd = new SqlCommand(insertCommand, connection);
    
        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }
    
        cmd.ExecuteNonQuery();
    }
 

In dit voorbeeld is bedoeld voor het weergeven van de basisprincipes. Een meer realistische scenario zou doorlopen met de vereiste diensten om de query-tekenreeks en de opdrachtparameters tegelijkertijd samen te stellen. U bent beperkt tot een totaal van 2100 query-parameters, zodat dit beperkt het aantal rijen dat op deze manier kan worden verwerkt.

De prestaties van dit soort insert-instructie weergegeven de testresultaten van de ad-hoc in milliseconden.

| Bewerkingen | Tabelwaarden parameters (ms) | Één instructie INSERT (ms) |
|---|---|---|
| 1 | 32 | 20 |
| 10 | 30 | 25 |
| 100 | 33 | 51 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de resultaten van de timing in dit onderwerp](#note-about-timing-results-in-this-topic).

Deze benadering is enigszins sneller voor de batches die minder dan 100 rijen zijn. Hoewel de verbetering klein is, is dit een andere optie die u wellicht in uw scenario voor de specifieke toepassing.

### <a name="dataadapter"></a>DataAdapter
De klasse **DataAdapter** kunt u een **DataSet** -object wijzigt en vervolgens de wijzigingen tijdens het invoegen, bijwerken en verwijderen in te dienen. Als u de **DataAdapter** op deze manier gebruikt, is het belangrijk te weten dat afzonderlijk worden aangeroepen voor elke afzonderlijke bewerking. Om prestaties te verbeteren, gebruikt u de eigenschap **UpdateBatchSize** om het aantal bewerkingen dat moet een batch worden geplaatst op een tijdstip. Voor meer informatie Zie [Uitvoeren Batch bewerkingen met behulp van DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Entiteit-framework
Entiteit-Framework ondersteunt momenteel geen batchen. Andere ontwikkelaars in de Gemeenschap hebt aantonen oplossingen, zoals de methode **SaveChanges** override geprobeerd. Maar de oplossingen zijn meestal complex en aangepast aan de toepassing en het gegevensmodel. Het kader van de entiteit codeplex project heeft momenteel een discussiepagina op het verzoek van deze functie. Deze discussie, Zie [Notulen ontwerp – 2 augustus 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
Voor de volledigheid voelen we is het belangrijk om te praten over XML als een batchen strategie. Het gebruik van XML heeft echter geen voordelen ten opzichte van andere methoden en verschillende nadelen. De aanpak is vergelijkbaar met de tabelwaarden parameters, maar een XML-bestand of tekenreeks wordt doorgegeven aan een opgeslagen procedure in plaats van een door de gebruiker gedefinieerde lijst. De opgeslagen procedure analyseert de opdrachten in de opgeslagen procedure.

Er zijn enkele nadelen aan deze benadering:

- Werken met XML kan erg onhandig zijn en foutgevoelig.
- Bij het parseren van het XML-bestand in de database zijn CPU-intensieve.
- Deze methode is in de meeste gevallen langzamer dan tabelwaarden parameters.

Het gebruik van XML voor batch-query's wordt niet aanbevolen om deze redenen.

## <a name="batching-considerations"></a>Overwegingen met betrekking tot batchen
De volgende secties bevatten meer richtlijnen voor het gebruik van deze batchverwerking in SQL-Database-toepassingen.

### <a name="tradeoffs"></a>Gebruik van systeembronnen
Afhankelijk van uw architectuur, batchen kan betrekking hebben op een balans tussen prestaties en tolerantie. Neem bijvoorbeeld het scenario waar uw rol onverwacht uitvalt. Als u één rij met gegevens verliest, is het effect kleiner dan de impact van het verlies van een grote reeks rijen niet-verzonden. Er is een groter risico wanneer u rijen van de buffer voordat deze worden verzonden naar de database in een venster.

Evalueren door deze verhouding, het type bewerkingen dat u batch. Batch-actiever (grotere batches en langere tijd windows) met gegevens die minder belangrijk zijn.

### <a name="batch-size"></a>Batchgrootte
In onze tests is er meestal geen voordeel van grote batches in kleinere stukken verdelen. In feite geleid dit deelvak vaak trager dan het verzenden van één grote batch. Neem bijvoorbeeld een scenario waar u 1000 rijen invoegen. De volgende tabel ziet u hoe lang het duurt om de tabelwaarden parameters gebruiken voor het invoegen van 1000 rijen wanneer onderverdeeld in kleinere batches.

| Batchgrootte | Herhalingen | Tabelwaarden parameters (ms) |
| -------- | --- | --- |
| 1000 | 1 | 347 |
| 500 | 2 | 355 |
| 100 | 10 | 465 |
| 50 | 20 | 630 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de resultaten van de timing in dit onderwerp](#note-about-timing-results-in-this-topic).

U kunt zien dat de beste prestaties voor 1000 rijen ze allemaal tegelijk verzenden. In andere tests (hier niet getoond) is er een kleine prestatie winst een batch 10000 rij opsplitsen in twee partijen van 5000. Maar het tabelschema voor deze tests is relatief eenvoudig, zodat u tests uitvoert op uw specifieke gegevens en -batch formaten om te controleren of deze bevindingen.

Een andere factor om te overwegen is dat als de totale batch te groot wordt, SQL-Database kan beperken en doorvoeren van de partij geweigerd. Test uw specifieke scenario om te bepalen of er een ideale batchgrootte voor de beste resultaten. De batchgrootte configureerbaar maken tijdens runtime om snel correcties op basis van de prestaties of fouten.

Ten slotte, de grootte van de partij in evenwicht met de risico's van de batchverwerking. Als er fouten van voorbijgaande aard of de functie is mislukt, kunt u de gevolgen van het opnieuw of verlies van de gegevens in de batch.

### <a name="parallel-processing"></a>Parallelle verwerking
Wat gebeurt er als u vond de aanpak van de vermindering van de omvang van de partij, maar meerdere threads gebruikt voor het uitvoeren van het werk? Nogmaals, onze tests bleek dat verschillende kleinere multithreaded batches meestal uitgevoerd erger dan een grotere partij. De volgende test probeert 1000 rijen invoegen in een of meer parallelle batches. Deze test toont aan hoe meer simultane batches daadwerkelijk afgenomen prestaties.

| Batchgrootte [aantal] | Twee threads (ms) | Vier threads (ms) | Zes threads (ms) |
| -------- | --- | --- | --- |
| 1000 [1] | 277 | 315 | 266 |
| 500 [2] | 548 | 278 | 256 |
| 250 [4] | 405 | 329 | 265 |
| 100 [10] | 488 | 439 | 391 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de resultaten van de timing in dit onderwerp](#note-about-timing-results-in-this-topic).

Er zijn verschillende mogelijke redenen voor de verminderde prestaties ten gevolge van parallellisme:

- Er zijn meerdere gelijktijdige netwerk oproepen in plaats van één.
- Meerdere bewerkingen op een enkele tabel kunnen leiden tot bronconflicten en blokkeren.
- Er zijn gekoppeld aan overheadkosten multithreading.
- Het voordeel van parallelle verwerking belangrijker is dan de kosten van meerdere verbindingen.

Wanneer u verschillende tabellen of databases, is het mogelijk om te zien krijgen met deze strategie prestaties. Database sharding of overkoepelende organisaties zou een scenario voor deze benadering zijn. Sharding maakt gebruik van meerdere databases en andere gegevens stuurt naar elke database. Als elke kleine partij naar een andere database gaat, kan vervolgens de bewerkingen uit te voeren parallel efficiënter zijn. De prestatieverbetering is echter niet significant te gebruiken als basis voor een besluit sharding database gebruiken in uw oplossing.

In sommige ontwerpen, parallelle uitvoering van kleinere batches kan leiden tot verbeterde doorvoer van aanvragen in een systeem onder belasting. In dit geval heeft weliswaar sneller verwerken van een grotere partij, meerdere batches in parallelle verwerking mogelijk efficiënter.

Als u parallelle uitvoering, kunt u het maximum aantal worker-threads te beheren. Een kleiner getal kan leiden tot minder bronconflicten en een sneller worden uitgevoerd. Ook kunt u de extra belasting die door deze op de doeldatabase in verbindingen en transacties geplaatst.

### <a name="related-performance-factors"></a>Verwante prestatiefactoren
Typische leidraden voor de prestaties van de database is ook van invloed op de batchverwerking. Voeg bijvoorbeeld prestaties voor tabellen die een grote, primaire sleutel of veel niet-geclusterde indexen wordt verminderd.

Als een opgeslagen procedure parameters tabelwaarden, kunt u de opdracht **SET NOCOUNT ON** aan het begin van de procedure. Deze verklaring wordt onderdrukt de terugkeer van het aantal betrokken rijen in de procedure. Echter, in onze tests, het gebruik van **SET NOCOUNT ON** geen invloed heeft of nemen de prestaties af. De proef opgeslagen procedure is eenvoudig met een enkele **plaats** van de parameter waarde tabel. Het is mogelijk dat meer complexe opgeslagen procedures kunnen van deze instructie profiteren zouden. Maar niet wordt ervan uitgegaan dat u **SET NOCOUNT ON** toevoegt aan de opgeslagen procedure worden de prestaties verbeterd. Om het effect te begrijpen, test u de opgeslagen procedure met en zonder de instructie **SET NOCOUNT ON** .

## <a name="batching-scenarios"></a>Batchen van scenario 's
In de volgende secties wordt beschreven hoe u de tabelwaarden parameters gebruiken in drie scenario's van toepassing. Het eerste scenario ziet u hoe een buffer en batchen kunnen samenwerken. Het tweede scenario verbetert de prestaties door de hoofd-en detailsectie bewerkingen uitvoeren in één opgeslagen procedure-aanroep. Het laatste scenario laat zien hoe tabelwaarden parameters gebruiken in een 'UPSERT'-bewerking.

### <a name="buffering"></a>Buffer
Er zijn enkele scenario's die voor de hand liggende kandidaat voor batchverwerking, zijn er veel scenario's die profiteren kunnen van de batchverwerking door vertraagde verwerking. Vertraagde verwerking wordt echter ook een groter risico dat de gegevens verloren gegaan in het geval van een onverwachte fout opgetreden zijn. Het is belangrijk dat dit risico begrijpt en rekening houden met de gevolgen.

Neem bijvoorbeeld een webtoepassing die de navigatiegeschiedenis van elke gebruiker worden bijgehouden. Bij elke aanvraag pagina kan de toepassing een database aanroepen om vast te leggen van de gebruiker paginaweergave maken. Maar hogere prestaties en schaalbaarheid kunnen worden bereikt door het bufferen van de gebruikers navigatie activiteiten en vervolgens deze gegevens worden verzonden naar de database in batches. U kunt de database bijwerken door de verstreken tijd en/of buffergrootte activeren. Een regel kan bijvoorbeeld opgeven dat de partij na 20 seconden of als de buffer bereikt 1000 items moet worden verwerkt.

In het volgende voorbeeld maakt gebruik van [Reactieve Extensions - Rx](https://msdn.microsoft.com/data/gg577609) gebufferde gebeurtenissen door een controle klasse verwerken. Wanneer u de buffer of een time-out is bereikt, de partij van de gegevens van de gebruiker wordt verzonden naar de database met een parameter tabelwaarden.

De volgende NavHistoryData-klasse modellen navigatie gegevens van de gebruiker. Het bevat essentiële informatie voor de gebruikers-id en de URL die wordt geopend de toegangstijd.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

De klasse NavHistoryDataMonitor is verantwoordelijk voor het bufferen van de gebruiker navigatie gegevens naar de database. Het bevat een methode, RecordUserNavigationEntry, die door een **OnAdded** -gebeurtenis reageert. De volgende code toont de logica van de constructor die Rx gebruikt voor het maken van een waarneembare collectie op basis van de gebeurtenis. Het abonnement neemt aan deze waarneembare-collectie met de methode Buffer. De overbelasting geeft aan dat de buffer moet worden verzonden om de 20 seconden of 1000 vermeldingen.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

De handler alle gebufferde items worden omgezet in een type tabelwaarden en dit wordt doorgegeven aan een opgeslagen procedure die de batch verwerkt. De volgende code bevat de volledige definitie voor zowel de NavHistoryDataEventArgs als de NavHistoryDataMonitor-klassen.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }
    
    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;
    
        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }
    
        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }
    
        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }
    
            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();
    
                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;
    
                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });
    
                cmd.ExecuteNonQuery();
            }
        }
    }

Deze buffer als klasse wilt gebruiken, maakt de toepassing een statisch object in de NavHistoryDataMonitor. Telkens wanneer die een gebruiker toegang heeft tot een pagina, roept de toepassing de methode NavHistoryDataMonitor.RecordUserNavigationEntry. De logica van de buffer verloopt zorgen deze posten te verzenden naar de database in batches.

### <a name="master-detail"></a>Hoofdsectie detailsectie
Tabelwaarden parameters zijn handig voor eenvoudige scenario's voor invoegen. Het kan zijn moeilijker te batch ingevoegd die betrekking hebben op meer dan één tabel. Het scenario "hoofd-/ detailsectie" is een goed voorbeeld. De hoofdtabel geeft de primaire entiteit. Een of meer detailtabellen meer gegevens over de entiteit worden opgeslagen. Externe-sleutelrelaties afdwingen in dit scenario wordt de relatie van de details van een uniek model entiteit. U kunt een vereenvoudigde versie van een PurchaseOrder-tabel en de bijbehorende OrderDetail-tabel. De volgende Transact-SQL wordt gemaakt van de PurchaseOrder-tabel met vier kolommen: ordernummer, orderdatum, KlantId en Status.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Elke order bevat een of meer kopen van een product. Deze informatie wordt opgenomen in de tabel PurchaseOrderDetail. De volgende Transact-SQL wordt gemaakt van de PurchaseOrderDetail-tabel met vijf kolommen: OrderDetailID, OrderID, ProductID, UnitPrice en OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

De order-id-kolom in de tabel PurchaseOrderDetail verwijst naar een order uit de tabel PurchaseOrder. Deze beperking zorgt ervoor dat de volgende definitie van een refererende sleutel.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Om de tabelwaarden parameters gebruikt, moet u een door de gebruiker gedefinieerde tabeltype voor elke doeltabel hebben.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO
    
    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Definieer vervolgens een opgeslagen procedure die in de tabellen van deze typen accepteert. Met deze procedure kunt lokaal batch-een reeks orders en Orderinformatie in één aanroep van een toepassing. De volgende Transact-SQL biedt de verklaring van de opgeslagen procedure voltooid voor dit voorbeeld inkooporders.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;
    
    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );
     
          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;
    
    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;
    
    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

In dit voorbeeld wordt de lokaal gedefinieerde @IdentityLink tabel bevat de werkelijke waarden voor order-id van de nieuw ingevoegde rijen. Deze order-id's wijken af van de tijdelijke waarden voor order-id in de @orders en @details tabelwaarden parameters. Om deze reden, de @IdentityLink tabel vervolgens verbindt de order-id van de @orders -parameter voor de werkelijke waarde van de order-id voor de nieuwe rijen in de tabel PurchaseOrder. Na deze stap, de @IdentityLink tabel invoegen van de gegevens van de met de feitelijke order-id die voldoet aan de refererende-sleutelbeperking kan vergemakkelijken.

Deze opgeslagen procedure kan worden gebruikt vanuit de code of vanuit andere Transact-SQL-aanroepen. Zie de sectie parameters tabelwaarden van dit artikel voor een voorbeeld van code. De volgende Transact-SQL ziet u hoe aanroepen van de sp_InsertOrdersBatch.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType
    
    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')
    
    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)
    
    exec sp_InsertOrdersBatch @orders, @details

Deze oplossing kan iedere partij gebruik van een reeks waarden voor order-id die bij 1 beginnen. Deze tijdelijke waarden voor order-id de relaties in de batch wordt beschreven, maar de werkelijke waarden voor order-id op het moment van de invoegbewerking worden bepaald. U kunt meerdere keren dezelfde instructies in het vorige voorbeeld worden uitgevoerd en genereren van unieke orders in de database. Om deze reden overwegen toe te voegen meer logica code of database waardoor dubbele orders met deze techniek batchen.

In dit voorbeeld wordt gedemonstreerd dat nog complexer databasebewerkingen, zoals het hoofd-en detailsectie bewerkingen, batch kunnen worden verzameld met behulp van tabelwaarden parameters.

### <a name="upsert"></a>UPSERT
Een ander scenario voor batchen moet gelijktijdig bijwerken van bestaande rijen en nieuwe rijen invoegen. Deze bewerking wordt soms aangeduid als een bewerking 'UPSERT' (update + insert). In plaats van afzonderlijke oproepen invoegen en bijwerken, de instructie samenvoegen meest geschikt is voor deze taak. De instructie samenvoegen kunt beide invoegen en bijwerken van bewerkingen in één aanroep.

Tabelwaarden parameters kunnen worden gebruikt met de instructie samenvoegen uit te voeren, bijgewerkt of ingevoegd. Stel dat u een eenvoudige tabel met medewerkers de volgende kolommen bevat: werknemer-id, de voornaam, achternaam, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))
 
In dit voorbeeld kunt u het feit dat de SocialSecurityNumber uniek is voor het samenvoegen van meerdere werknemers. Maak eerst de door de gebruiker gedefinieerde tabeltype:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Vervolgens maakt u een opgeslagen procedure of code schrijven die de instructie samenvoegen gebruikt de update uitvoeren en invoegen. Het volgende voorbeeld wordt de instructie voor het samenvoegen van een parameter tabelwaarden @employees, van het type EmployeeTableType. De inhoud van de @employees tabel worden hier niet weergegeven.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Zie de documentatie en voorbeelden voor het samenvoegen-overzicht voor meer informatie. Hoewel het hetzelfde werk kan worden uitgevoerd in een multiple-stap opgeslagen procedure-aanroep met afzonderlijke invoegen en bewerkingen, de instructie samenvoegen is efficiënter. Databasecode kan ook Transact-SQL-aanroepen met de instructie samenvoegen rechtstreeks zonder twee database-aanroepen voor INSERT en UPDATE samenstellen.

## <a name="recommendation-summary"></a>Overzicht van aanbevelingen

De volgende lijst bevat een overzicht van de batchen aanbevelingen die in dit onderwerp worden besproken:

- Buffering en batchen gebruiken om de prestaties en schaalbaarheid van SQL-Database-toepassingen.
- Inzicht in de wisselwerking tussen batchen/bufferen en tolerantie. Tijdens een mislukte rol het risico van het verlies van een niet-verwerkte partij van bedrijfskritieke gegevens de prestatievoordelen van batchen zwaarder wegen dan.
- Proberen te houden van alle oproepen naar de database in een enkele datacenter te verminderen latentie.
- Als u een bepaalde techniek batchen, worden parameters tabelwaarden bieden de beste prestaties en flexibiliteit.
- Volg deze algemene richtlijnen voor de snelste prestaties invoegen, maar uw scenario testen:
    - Gebruik een enkele parameters voor < 100 rijen, opdracht invoegen.
    - Voor < 1000 rijen, tabelwaarden parameters te gebruiken.
    - Voor > = 1000 rijen, SqlBulkCopy gebruiken.
- Voor bijwerken en verwijderen, tabelwaarden parameters gebruiken in opgeslagen procedure logica die de juiste bewerking op elke rij in de tabel parameter bepaalt.
- Richtlijnen voor batch grootte:
    - Gebruik de grootste batch grootte die zinvol zijn voor uw toepassing en de zakelijke vereisten.
    - Het saldo van de winst van de prestaties van grote batches met de risico's van tijdelijke of onherstelbare fouten. Wat is het gevolg is van pogingen of het verlies van de gegevens in de batch? 
    - Test de grootste batchgrootte om te controleren of SQL-Database wordt niet weigeren.
    - Configuratie-instellingen maken dat besturingselement batchen, zoals de grootte van de partij of het tijdvenster buffering. Deze instellingen bieden flexibiliteit. U kunt de batchen gedrag in productie wijzigen zonder de cloud-service opnieuw te implementeren.
- Parallelle uitvoering van batches die worden uitgevoerd op een enkele tabel in een database voorkomen. Als u kiest om een partij te verdelen over meerdere worker-threads, tests om te bepalen van het ideale aantal threads worden uitgevoerd. Na een niet-gespecificeerde drempel meer threads zullen de prestaties verminderen in plaats van verhogen.
- U kunt buffer grootte en tijd bij wijze van uitvoering van de batchverwerking voor meer scenario's.

## <a name="next-steps"></a>Volgende stappen

In dit artikel gericht op hoe databaseontwerp en gerelateerd aan batchen coderingstechnieken de prestaties en schaalbaarheid verbeteren kunnen. Maar dit is slechts één factor in de algemene strategie. Zie voor meer informatie over het verbeteren van prestaties en schaalbaarheid [Azure SQL Database prestaties richtlijnen voor enkele databases](sql-database-performance-guidance.md) en [Overwegingen voor prijs- prestatieverhouding voor een elastische database-toepassingen](sql-database-elastic-pool-guidance.md).
