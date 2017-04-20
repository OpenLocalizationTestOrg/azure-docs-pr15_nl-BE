<properties
   pageTitle="U SQL scripts schrijven met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio | Azure"
   description="Informatie over gegevens Lake Tools installeren voor Visual Studio, het ontwikkelen en U SQL-testscripts. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Zelfstudie: Aan de slag met Azure gegevens Lake Analytics U SQL-taal

U SQL is een taal die de voordelen van SQL met de expressieve voordelen van uw eigen code verenigt voor het verwerken van alle gegevens op een schaal. U-SQL schaalbare, gedistribueerde query-mogelijkheid kunt u efficiënt om gegevens te analyseren in de winkel en over de relationele winkels zoals Azure SQL-Database.  U kunt naar proces ongestructureerde gegevens volgens schema op lezen, invoegen van aangepaste logica en de UDF en uitbreidbaarheid zodat u probleemloos korrelig controle over het uitvoeren van schaal bevat. Raadpleeg voor meer informatie over de ontwerpfilosofie achter U SQL, deze [Visual Studio van blogberichten](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Er zijn enkele verschillen van de ANSI SQL- of T-SQL. De sleutelwoorden zoals SELECT moeten bijvoorbeeld worden in hoofdletters.

Het type systeem en expressie taal in de select-component, waar de predicaten enz in C# zijn's.
Dit betekent dat de gegevenstypen zijn de C#-typen en de gegevenstypen C# NULL semantiek en de vergelijking van bewerkingen binnen een predikaat Volg C#-syntaxis (bijv. een == "foo").  Dit betekent ook, dat de waarden zijn volledige .NET-objecten, zodat u gemakkelijk een methode gebruiken om te werken met het object (bijvoorbeeld "f o o o". Split(' ')).

Zie voor meer informatie [Over U SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).

###<a name="prerequisites"></a>Vereisten

Voltooi eerst [Zelfstudie: U SQL scripts schrijven met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

In de zelfstudie, kunt u een taak gegevens Lake Analytics uitgevoerd met de volgende U SQL-script:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

Dit script is geen transformatie stappen. Worden gelezen vanuit het bronbestand genoemd **SearchLog.tsv**, het schematizes, en de rijenset wordt de uitvoer in een bestand met de naam **SearchLog-eerste-u-sql.csv**.

Let op het vraagteken naast het gegevenstype van het veld Duur. Dit betekent dat het veld duur kan niet null zijn.

Sommige concepten en trefwoorden die in het script:

- **De rijenset variabelen**: elke query-expressie die een rijenset produceert kan worden toegewezen aan een variabele. U SQL volgt de T-SQL variabele naampatroon, bijvoorbeeld, **@searchlog** in het script.
    Opmerking de toewijzing niet voor dat kan worden uitgevoerd. Alleen de naam van de expressie en biedt u de mogelijkheid om de opbouw meer complexe expressies.
- **Uitpakken** kunt u een schema instellen op lezen. Het schema is opgegeven door een kolomnaam en C# type naam paar per kolom. Een zogenaamde **Extractor**, bijvoorbeeld **Extractors.Tsv()** tsv-bestanden uitpakken wordt gebruikt. U kunt aangepaste extractie ontwikkelen.
- **Uitvoer** wordt een rijenset en het serialiseert. De Outputters.Csv() de uitvoer van een CSV-bestand in de opgegeven locatie. U kunt ook aangepaste Outputters ontwikkelen.
- U ziet dat twee paden zijn relatieve paden. Ook kunt u absolute paden.  Bijvoorbeeld

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Absoluut pad hebt u toegang tot de bestanden in de gekoppelde accounts voor opslag.  De syntaxis voor bestanden die zijn opgeslagen in de gekoppelde Azure opslag account is:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob container met openbare BLOB's of machtigingen voor openbare containers worden momenteel niet ondersteund.

## <a name="use-scalar-variables"></a>Scalaire variabelen gebruiken

U kunt scalaire variabelen en vereenvoudigen het onderhoud van uw script. Het vorige U SQL-script kan ook worden geschreven als het volgende:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Rijensets transformeren

Gebruik **selecteren** om te transformeren rijensets:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

De WHERE-component gebruikt de [C# Boole-expressie](https://msdn.microsoft.com/library/6a71f45d.aspx). De C# taal voor sjabloonexpressies kunt u uw eigen expressies en functies. U kunt zelfs uitvoeren meer gecompliceerde filteropdrachten ze te combineren met logische voegwoorden (en) en disjunctions (ORs).

Het volgende script wordt de methode DateTime.Parse() en een combinatie.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

U ziet dat de tweede query van het resultaat van de eerste rijenset werkt en het resultaat dus een samenstelling van deze twee filters is. U kunt ook de naam van een variabele hergebruiken en de namen zijn binnen het bereik van lexically.

## <a name="aggregate-rowsets"></a>Statistische rijensets

U SQL biedt de vertrouwde **ORDER BY**, **GROUP BY** en samenvoegingen.

De volgende query vindt u de totale duur per regio en vervolgens boven uitgang 5 duur in volgorde.

U SQL rijensets blijven niet behouden in de volgorde van de volgende query. Om de output van een order, moet u dus ORDER BY toevoegen aan de instructie OUTPUT zoals hieronder wordt weergegeven:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

U SQL ORDER BY-component moet worden gecombineerd met de FETCH-component in een SELECT-expressie.

DAT U SQL-component kan worden gebruikt voor het beperken van de uitvoer aan groepen die voldoen aan de HAVING-voorwaarde:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Gegevens koppelen

U SQL biedt algemene join-operatoren zoals INNER JOIN, rechts-links/FULL OUTER JOIN, SEMI-JOIN join niet alleen tabellen, maar alle rijensets (ook die geproduceerd zijn uit bestanden).

Het volgende script wordt toegevoegd aan de searchlog met een advertentie indruk logboek en geeft ons de reclames voor de query-tekenreeks voor een bepaalde datum.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


U SQL biedt alleen ondersteuning voor de ANSI-compatibele join-syntaxis: Rowset1 JOIN Rowset2 ON predikaat. De syntaxis van de oude van van Rowset1, waarbij Rowset2 predikaat wordt niet ondersteund.
Het predikaat in een JOIN is een join gelijkheid en geen expressie. Als u gebruiken een expressie wilt, toe te voegen aan een vorige rijenset select-component. Als u een andere vergelijking doen wilt, kunt u deze verplaatsen in de WHERE-component.


## <a name="create-databases-table-valued-functions-views-and-tables"></a>Databases, tabelwaarden functies, weergaven en tabellen maken

U SQL kunt u gegevens in de context van een database en een schema gebruiken. U hebt dus geen altijd lezen uit of schrijven naar bestanden.

Elk U SQL-script wordt uitgevoerd met een standaarddatabase (master) en een standaardschema (DBO) als de standaardcontext. U kunt uw eigen database en/of schema maken. Als u de context wijzigen, gebruikt u de instructie **gebruiken** voor het wijzigen van de context.


### <a name="create-a-table-valued-function-tvf"></a>Maak een tabelwaardefunctie (TVF)

In het vorige U SQL-script herhaald u met lezen van het bronbestand dezelfde EXTRACT. U SQL tabelwaardefunctie kunt u voor het inkapselen van de gegevens voor toekomstig gebruik.   

Het volgende script wordt een *Searchlog()* genoemd in de standaarddatabase en het schema TVF gemaakt:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

Het volgende script laat zien hoe u de TVF gedefinieerd in het vorige script gebruiken:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Weergaven maken

Als u slechts één query-expressie die u wilt als abstract en niet wilt dat voorzien, kunt u een weergave in plaats van een tabelwaardefunctie.

Het volgende script wordt een weergave met de naam *SearchlogView* in de standaarddatabase en het schema gemaakt:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Het volgende script laat zien met de gedefinieerde weergave:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Tabellen maken

U SQL is vergelijkbaar met een relationele database, tabel, kunt u een tabel maken met een vooraf gedefinieerd schema of een tabel maken en het schema van de query die de tabel (ook bekend als maken tabel AS SELECT of CTAS) gevuld afgeleid.

Het volgende script maakt een database en twee tabellen:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Querytabellen

U kunt de tabellen (gemaakt in het vorige script) op dezelfde manier als u query opvragen via de gegevensbestanden. In plaats van een rijenset EXTRACT gebruik maken, kunt nu u alleen verwijzen naar de naam van de tabel.

Het transform-script dat u eerder hebt gebruikt wordt om te lezen uit de tabellen gewijzigd:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Houd er rekening mee dat u momenteel niet uitvoeren een selectie op een tabel in hetzelfde script als het script waarin u die tabel maken.


##<a name="conclusion"></a>Conclusie

Wat vindt u in de zelfstudie is slechts een klein deel van U SQL. Vanwege de omvang van deze zelfstudie, het niet kan betrekking hebben op Alles, zoals:

- Gebruik CROSS APPLY delen van tekenreeksen, matrices en kaarten uitpakken in rijen.
- Gepartitioneerde sets gegevens (bestanden en gepartitioneerde tabellen) worden uitgevoerd.
- Door gebruiker gedefinieerde operators zoals extractie, outputters, processors, door de gebruiker gedefinieerde aggregators in C# ontwikkelen.
- U SQL windowing functies gebruiken.
- U SQL-code met weergaven, functies tabelwaarden en opgeslagen procedures beheren.
- Willekeurige aangepaste code kan uitvoeren op de verwerking van knooppunten.
- Verbinding maken met Azure SQL-Databases en federate query's over hen en uw gegevens U SQL en Azure gegevens Lake.

## <a name="see-also"></a>Zie ook

- [Overzicht van Microsoft Azure gegevens Lake Analytics](data-lake-analytics-overview.md)
- [U SQL scripts schrijven met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Gebruikt U SQL venster functies voor Azure gegevens Lake Analytics taken](data-lake-analytics-use-window-functions.md)
- [Controleren en oplossen van problemen met Azure gegevens Lake Analytics taken met Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Laat ons weten wat u denkt

- [Het verzoek om een functie indienen](http://aka.ms/adlafeedback)
- [Vraag hulp in de forums](http://aka.ms/adlaforums)
- [Feedback over U SQL](http://aka.ms/usqldiscuss)
